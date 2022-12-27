# RTA- Fuel-on-demand
Refuel Estimation of the haul trucks.
__Fuel on Demand for Amrun Site__


__Summary__:
* Fuel on demand project aims at effective refuel time estimation for the haul trucks to avoid the existed pre-mature static refueling of the trucks and re fuel the trucks whenever it is necessary i.e., only fuel when truck is down to ~10% of tank capacity.
* With the static refueling system, every truck must go for a refuel after every 34 hours of last refuel event, which leads to pre-mature refueling of most of the trucks and this premature refilling system leads to unnecessary wait time at refuel station for other trucks. This leads to wastage of operational hours, which otherwise could have been utilized in haul operations  
* Dynamic refueling system estimates the refueling of the haul trucks, by dynamically estimating the time by which it would run out of fuel based on the operational data that

__Schedule job run__:
* Model development is done on historical data of around 42 million records collected over a month from September 6, 2021 till October 6, 2021. Model is saved in a file to be used for future estimations.
* Estimation job run to take place for every 5 minutes and simultaneously, power BI dashboard will be refreshed to give the latest updates of the haul trucks.
* Real-time data is being retrieved from two separate database servers for the last 7 days every time it runs, and data is combined to perform the desired estimation.

__Script Flow__:
* Fod_constants.py is used to store all the constants used in the script, including the sensitive credentials of the servers from where data is being retrieved.
* Fod_utils is used to maintain the database connections.
* Fod_functions is used to perform the model building operation as well as live estimations based on the model.
* Requirements.txt is used to maintain all the dependencies (libraries) of the python script.
* Main.py is the main script which runs every 5 minutes to estimate the fuel consumed, the duration in which it is consumed, next refuel time, time left for refuel event etc. and update the current status on the power BI dashboard as well.
* LIVE_TIME_USAGE_4.sql is the sql script to retrieve the dynamic time usage data of the haul trucks. 
* FMSCORE_BANLAW_2.sql is the sql script to retrieve the historical data of refuel events of the haul trucks and how much they were refueled.

__Database Schema__:

| Schema	| Description |
| ------------- |------------- |
| bauxite_fod | Estimations for Amrun Site |

__Database Tables__:

| Tables	| Description |
| ------------- |------------- |
| estimates	| Contains all the refuel estimations |
| Dashboard	| Contains power BI dashboard related modifications in the estimates|
| estimates_archive | Contains all the refuel estimation of last 3 months |
| data_log | Contains log of all the operations |

__AWS Deployment__:
* Python code developed for the Project is tested in local environment. So, created seperate folder called Lambda which contains AWS Lambda Version.
* Once changes are tested, We need to Zip the contents and push it to S3 bucket (s3://rta-fuel-on-demand-bucket/lambda/fod_lambda.zip).  
* Main build pipeline is pipelines/infra/fod-prod-deploy.yml and all the required resources are in templates/aws/fod-function.yml
* Cross check if any changes required in pipeline and if not then just re-deploy the pipeline (https://dev.azure.com/PACE-DevOps/ODE-RTA-Core/_build?definitionId=524) in Azure Devops.


