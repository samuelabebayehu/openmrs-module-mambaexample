
OpenMRS MambaETL Reporting Module
==========================  

Amendments to base openmrs platform module generated
-----------  
This is a starting point module which has basic dependencies' setup:  
1. **POM files update:**
- *Inside parent POM XML* : the build definitions are updated to orchestrate creation of required Stored Procedures,  All required dependencies are added including mamba core dependency and properties file updated accordingly.
- *Inside OMOD POM XML* : build lines are updated
- *Inside api POM XML* : new dependencies are added to reflect mamba dependency
2. **Inside OMOD folder**
- Created folder `_etl` inside `/omod/src/main/resources` and added two directories `config` and `derived`
- copied `sp_makefile` and `sp_mamba_data_processing_etl.sql` to `_etl` folder files from official mamba etl repo as a starting point
- Under resources folder inside `config.xml` added  line
3. **Inside API folder**
- Updated `liquibase.xml` to run the generated `create_stored_procedures.sql`
- Updated `moduleApplicationContext.xml` file to remove contents except `<context:component-scan base-package="org.openmrs.module.mambaetl" />`

Setting up your project
--------------------  
4. Clone the repo and rename the project
5. Go to your existing database and run below query replacing `encounter_type.name` value with required encounter type [Generate flat table JSON](https://pastebin.com/K31iBWNK)
6. Copy the value inside `/omod/src/main/resources/_etl/config` eg. If you have generated JSON for followup you can paste it to the existing file or rename the file and paste the contents. To add more flat encounters repeat the process and create additional files

Installation
------------  
7. Update the main POM.XML file to set different DB arguments so the tables are generated in different schema and define a source DB to extract data from:
   ![Arguments](https://i.ibb.co/zX5QdkY/Screenshot-from-2024-07-16-12-19-39.png)


`The source database (OpenMRS database): -d openmrs_v2 `
`The target or analysis Database where the ETL data is stored: -a analysis_db_test`
8. Build the module to produce the .omod file and required sql files with `mvn clean install`.
9. To validate project without deployment first run the SQL file `create_stored_procedures.sql` this is a generated file from maven clean install.
10. Finally run `CALL sp_mamba_data_processing_flatten();` to load the flattened data accordingly.
11. If everything goes well you should see the tables inside schema `analysis_db_test` should be populated like `mamba_z_encounter_obs` 
  