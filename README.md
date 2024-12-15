Requirement: To read multiple files (buyers, sellers, countries, users) and manage them for visualization is required.

what we are doing?
We have all 4 files, buyers/sellers/countries data would not change so we can add it to the pipeline altogether. Users data would be continuously coming hence we would need to make seperate pipeline for it.

our structurs is usual medallion pattern:

Landing 1:  (data would be comintg to this zone first)
- user  (new data would be coming here)
- seller
- countries
- buyers
Landing 2:   (we will use azure pipeline to copy(csv-parquet) the data from landing zone 1 to landing zone 2)
- user   (create seperate pipeline which fetches new changes in Landing-1/user subdirectory. It might be a pipeline trigger to get new file and save in Landing-2/users
- seller
- countries
- buyers

Databricks:
we will mount the container (landing 2) to databricks to fetch the file as required:
we will create delta lakes for these. 
Bronze: Simply fetching the data on this layers from landing-zone-2 and save in bronze folder delta table
Silver: fetch data from Bqonze delta table , make some operations(whatever cleaning, transformation is required) and save the delta table in Silver layer
Gold: Fetch from silver layer and here we will do final changes in the table. Join all the tables to one-big-table maybe? From here visualization can be done if required.

The new data coming in Landing-2/user shall be processed as well. (will add details layer maybe) :)
