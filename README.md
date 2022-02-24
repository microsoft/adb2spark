# Guidance to migrate workloads from Azure Databricks to Synapse Spark
This repository contains information about common challenges one might face while migrating analytic workloads from ADB to Spark and provides resolutions to those.
1. HttpException issue
2. Tables which needs graphframes library (no PK, UK tables) are giving error 
3. Date Format Issue 
4. SQL Paas connection error: If we run multiple tables (more than 40) within one application. Synapse spark throws connection error, hence affecting degree of parallelism. 
 
