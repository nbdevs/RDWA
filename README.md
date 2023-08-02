# [Real-time Data Warehousing with Airflow](https://github.com/nbdevs/dag-development/files/12240734/MSc.Dissertation.1.pdf)

<div>
<img src="https://github.com/nbdevs/dag-development/assets/75015699/29abf1ff-dfe6-4d5f-a6bd-cbd761f900a4" title="Visualisation tool" alt="PowerBI" width="200" height=""/>&nbsp;
<img src=https://github.com/nbdevs/dag-development/assets/75015699/86549b0d-d8d3-4703-b624-ae04d181e9c1 title="Cloud provider for data warehouse" alt="Snowflake" width="200" height="" /> &nbsp;
<img src=https://github.com/nbdevs/dag-development/assets/75015699/55f0791c-02d1-42b9-a7da-3a006d3f67f2 title="Cloud provider for intermediary storage" alt="S3" width="200" height="" />&nbsp;</div>

The parent repository that stores the entirety of development for the real time data warehousing pipeline.

The project structure has separated development into two child repositories [__dag-development__](https://github.com/nbdevs/dag-development) and [__postgres-db-cluster__](https://github.com/nbdevs/postgres-db-cluster) as seen above, which by clicking on the two repositories respectively, further more detailed information about the relevant systems will be displayed.

---

## Product Perspective

The model decided upon to depict the interactions between the different actors participating within the system on a higher level was finalized as a Use Case diagram. This was to address separation of concerns. By isolating the actors used per use case per subsystem, focused strategies for software development were drafted which best represented these interactions by incorporating the requirements that are applicable to a system function for a specific use case.

<div>
<img width="659" alt="Use Case Diagram" src="https://github.com/nbdevs/dag-development/assets/75015699/226c2260-3bb1-4dc0-92b4-3ea07cd8d4c9">
</div>
The system displayed above consists of 5 actors and the 6 secondary actors which our system connects to via external interfaces all working within 3 separate components - the database system, ETL system, and BI system - to create the entire data warehousing system.

Details into the secondary actors represented in the model above are detailed below:

1. __AWS__: _“A team of programmers outside of our system which provide access to their IaaS products which will help with the building and managing of our ETL pipeline.”_
2. __FastF1 API__: _“The team of programmers outside of our system which provide the necessary F1 data required for our data pipeline.”_
3. __Snowflake__: _“An open-sourced data warehouse product provided by a team outside of our system which provides us with OLAP support.”_
4. __Airflow__: _“An external python package from a team of programmers outside of our system which provide support for monitoring and orchestration of ETL tasks”_
5. __Git__: _“A version control platform used for tracking developmental efforts.”_
6. __Docker__: _“A container register used for running multiple applications in one environment.”_

## Product Functionality

<div>
<img width="771" alt="Top Level Data Flow Diagram" src="https://github.com/nbdevs/dag-development/assets/75015699/0129cc49-4125-4455-89cc-a2ae00dfc1d1">
</div>

The __Top-Level Data Flow diagram__ shown above summarises the data flow associated with all the use cases.

These use cases represent major groups of related requirements within the overall system architecture, although the diagram mainly pertains towards the roles performed by the Data Engineer which are packaged within the airflow orchestration folder, as well as those performed by the Data Analyst to assist the Client which are contained in the analyst orchestration folder.

There is some reference to the functionality provided by the Database Administrator in  
reference to the stored procedures that must be in place to conform the data in accordance with the predefined schemas.

## User Classes and Characteristics

The functionality for the primary actors (user classes) modelled within the system are described below.

1. __Database Administrator__
A programmer responsible for maintaining the security and integrity within the system by preventing write access to the primary DBMS for unprivileged users, as well as initialising the database environments in both OLTP and OLAP layers. Primarily their role is to create the infrastructure for the developers to stream data into and out from. This user has unrestricted access to the database system CRUD functionality.”
1. __Client__
A person who wishes to view the visualizations pertaining to relevant questions they have about F1 from the perspective of a fan. This user has no access to the data warehousing system, except to see the products of the system – visualisations.
1. __Data Engineer__
A team of two programmers is responsible for the development of DAGs, SQL queries and ETL tasks:
    a) - The database developer populates the database after ingesting data from API and has access to the production 3NF database.
    b) - The data warehouse developer has read only access of the replicated main 3NF database. Although their privileges are elevated in accessing the data warehouse main production database with CRUD functionality.
1. __Data Analyst__
An analyst who prepares the visualisation dashboard using metrics the client is mostly concerned about. This user doesn’t have access to the database system, and only has read access of the data warehouse.

---

## Operating Environment

<div>
    <img src="https://github.com/nbdevs/dag-development/assets/75015699/f4d5b26b-2b80-4ad7-92af-8894571cf537" title="Software Overview" alt="software" width="413" height="90"/>&nbsp;
    <img src="https://github.com/nbdevs/dag-development/assets/75015699/fbc07632-8350-4869-aa3a-ab3e7aaef9af" title="Hardware" width="426" height="150" alt="hardware"/>&nbsp;
</div>

## Design and Implementation Constraints

|**Tool**|**Rationale**|
|--|--|
|__Airflow__|A workflow process is critical for ETL due to the requirements for consistency, because of dependencies between pre-requisite processes. Airflow models tasks as a unit of execution, and the directed acyclic graphs (DAGs) are composed of networks of tasks that are executed by operators and run in a predefined order.  The DAG capabilities of handling many tasks make it ideal for ETL pipeline management through DAG development, and so Airflow was selected as the workflow orchestration tool.|
|__PostgreSQL__|The abundance of developers within the open-sourced community is stronger within PostgreSQL than in mySQL for SQL development, which is an important factor. In addition to this, PostgreSQL has an object-relational database, in contrast, mySQL has a relational database. The object-relational database has advantages over relational as it enables file manipulation operations on JSON files which will be needed for improving query performance, and data lookup.|
|__VSCode__|VSCode was selected as the IDE for source development due to the ease of use, as well as the integration capabilities. The requirements of the project mean multiple languages are used, so it is useful have to all code on the same IDE for development.|
|__PowerBI__|PowerBI offers a free trial and an educational license accessibility after expiry, in addition to this it is cheaper than its more expensive direct competitor Tableau The prevalence of PowerBI within the BI stacks of organisations in industry made me opt for PowerBI over Tableau as although
tableau has superior visualisations, the capabilities of PowerBI are more extensive and include customizable visualisations and modelling capabilities superior to Tableau. These tabular modelling capabilities will be much more useful for utilising the star schema design effectively and capturing accumulating snapshot fact tables.|
|__AWS__|AWS was selected to be the cloud provider over Google and Azure, mainly because of AWS the most mature cloud ecosystem with them being the industry leader, and the wide range of functionality which is present all within the same ecosystem for integration.|
|__DBA – DW Architecture__|The Data Bus Architecture was implemented instead of the EDW due to the agile methodology imposed on the project, which sees to iteratively produce data marts with extra functionality on each new sprint. This way the enterprise warehouse can be summarised into data marts which are consistent for drill-across functionalities in the presentation layer.|
|__Jenkins__|I decided to use Jenkins for automation of the CI/CD process because it is free, open-source, and integrates well with my tech stack.|
