# Fabricator
CLJ is a (fictional) events management business. CLJ were hired back in 2023 by a big technology company to manage their user conferences: FabCon in Las Vegas and also now in Europe as well. 

You have been hired by CONFX as an Analytics Engineer to build a data warehouse to store, model and analyze data about the conferences. 

You will be guided step-by-step through a development process and be given different tasks to complete along the way. 



## Skills Developed

This project has been designed to help you build practical skills and experience in: 

Data cleaning and transformation using T-SQL 

Data modelling in a Data Warehouse

Ability to map messy real-world datasets into reusable datasets. 


## Initial environment (& setup) 

To set the scene, the project begins shortly after the second Fabric conference in Stockholm (end of September 2024). 

The conferences were a huge success, and you've got a load of data from the conferences to analyze - the only problem is that the data is in pretty raw form. 

A data engineer has managed to extract the datasets from a variety of source systems, and dumped all the datasets into a load of CSV files (how nice of him). 

You will first play the role of the data engineer and load all the CSV data into a Bronze Lakehouse (see environment setup below). Then for the rest of the project, you will assume the role of Analytics Engineer. 

## Environment setup (must complete‼️)

To get started, you will need to: 

Create a new Lakehouse (WITHOUT schemas enabled) in your Fabric Workspace, called DW018_BronzeLH.  

Download the set of starter files from the ZIP file at the bottom of this page. Unzip it, and load all the files into the Files area of your DW018_BronzeLH (put them all in the root Files/ directory, don't create any subfolders). 

Create a new Notebook from within that Lakehouse, and run the following code: 

files_to_load = [
    ("Files/CRMCustomers.csv", "em_customers"),
    ("Files/Offers.csv", "em_offers"),
    ("Files/Venues.csv", "em_venues"),
    ("Files/TicketSales.csv", "em_ticket_sales"),
    ("Files/ConferenceEvents.csv", "em_conference_events"),
    ("Files/LVAttendanceRecords.csv", "lv_attendance_records"),
    ("Files/LVAttendees_mapping.csv", "lv_attendees_mapping"),
    ("Files/LVRooms.csv", "lv_rooms"),
    ("Files/LVSchedule.csv", "lv_schedule"),
    ("Files/LVSpeakers.csv", "lv_speakers"),
    ("Files/STOCKCheck_Ins.csv", "stock_check_ins"),
    ("Files/STOCKSessions.csv", "stock_sessions"),
] 

for file_path, table_name in files_to_load: 
    df = spark.read.format("csv").option("header", "true").option("inferSchema", "true").load(file_path)
    df.write.mode("overwrite").saveAsTable(table_name)

Setup validation: 

You will pass this exercise when you achieved the following: 

✅ You should now have twelve Lakehouse tables, with data in each of them. You can navigate to the SQL analytics endpoint of your Lakehouse to inspect the tables using T-SQL. If you have data in all of them, congratulations, you have completed this environment setup, and you can begin the Project in the next tutorial. 

![image](https://github.com/user-attachments/assets/28ff7274-2839-4551-9e96-b86775ebc9b7)

## Business and data understanding

We will be using these tables a lot over the next tutorials, so take some time to explore the different tables, try to understand the data, try to understand how they might fit together (or not!), think about how some of the tables might be connected, or need transformation/ cleaning. 

Specifically, you should start thinking about: 
Which of these tables could represent Facts, and which could represent Dimensions (we'll cover Facts and Dimensions in the following tutorials)?
Which interesting business questions could you answer with the data? 
To help you out, the data engineer left you this sketch to help you understand the data available. Not exactly a data dictionary, but better than nothin

## Target architecture for this project

For this project, you will be implementing a layered architecture. In fact, in the setup above, you have already built the first two 'layers' of your architecture, the Landing Zone and the Bronze layer in your Lakehouse. 
At least initially (in this first set of tutorials), we will focus solely on building out the Green Layer (the one in the middle!). 

We will define our cleaned/ conformed layer with the following properties: 
- tables will be cleaned from the raw table to remove any erroneous data (and preferably validated, but we will not be validating our data in this module, we have a whole module on Data Quality validation coming up). 
- where appropriate (given the business context, and potential downstream modelling use cases), data from separate sources systems will be combined into single datasets (i.e. conformed) 
- our modelling at this stage will be generic enough to be used for a variety of downstream use cases, but we should have an idea of potential use cases for each table, and model them with these use cases in mind.
- 
I call this layer 'the middle'; when you begin to switch from a source-aligned mindset to a target-aligned mindset.

Conformed layer (medallion) vs Conformed dimension (Kimball)

Important terminology note: the Conformed Layer in a medallion architecture serves a different purpose to a Conformed Dimension, which you might be familiar with in Kimball dimensional modelling. 

A dataset in the Conformed Layer of a medallion architecture combines multiple datasets that relate to the same thing (normally from different source systems), into a single dataset.

For example, you could be storing data about Clients in a Client Management System and in your Project Management System. In a conformed layer, you could create a Clients dataset that brings together data from both systems. 

A Conformed Dimension (in Kimball dimension modelling) is a dimension table that connects two fact tables together. 

Why not just build your analytical models from your raw data? 

You might be asking: "why do we need this layer? Can't I just build my analytics models on top of the Raw tables?". 

You can do that, and it will be fine, up until a point. But as you build more and more analytical models, it's likely that a lot of them will repeat quite a lot of the business logic/ transformations, and sometimes repeat the business logic in slightly different ways, which can lead to different results/ numbers on the front-end, and angry stakeholders. 

Plus, it makes it very difficult to maintain, dozens of analytical models all doing some form of transforming the raw datasets. 

By building this generic layer of cleaned and conformed business data (you could call it Master Data), you are effectively creating a robust set of datasets that can be used as the foundation for multiple downstream analytical models.

