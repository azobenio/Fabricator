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

