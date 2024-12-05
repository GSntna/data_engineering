# Data Warehouses

## Index

## Basics

A data warehouse is a computer system designed
to store and analyze large amounts of data for
an organization.

* It gathers data from different areas
* Integrates and stores the data
* Make it available for analysis
* Supports business intelligence activity

### Data warehouses vs data lake

#### Data warehouse

* Stores structured data
* Gather data, integrate, makes it available for analysis
* Many input data sources
* Complex to change
* Typically >100 GB
* **Datamart**
  * It can be an extension of a *Data Warehouse*
  * A relational database for analysis
  * Data is focused on one subject area
  * Few input data sources
  * <100 GB

#### Data lake

* Stores structured and unstructured data (video, audio, 
  documents, etc.)
* Entire organization store of data
* Contains data from many departments
* Many data input sources
* Less complex to make changes
* Less organized
* Typically >100 GB

### Data warehouse cycle

1. **Planning**: involves requirements gathering and data
  modeling to understand organizational needs and how to
  structure the data warehouse.
2. **Implementation**: centers on designing the ETL process
  and developing BI applications to interact with the data
  warehouse.
3. **Support and maintenance**: focuses on updating the warehouse,
  testing, deployment, and making necessary changes to meet
  business requirements. 

## Architectures and properties

### Layers of data warehouse

1. **Data sources**:
2. **Data staging**:
   * Extracts, transforms and cleans data
   * Contains ETL process and storage tables
3. **Data storage**:
   * Data pipelines store the staging result in the data
    warehouse or data marts
4. **Data presentation**:
   * Users interact with stored data (BI, data mining,
      direct queries)

![data warehouse layers](./img/datawarehouse_layers.png)

### Architectures

#### Inmon Top-down approach

* The organization sets all the data definitions, cleaning,
  and business rules before any data enters the warehouse
* It stores data in a normalized forms
* After getting to the warehouse, the data moves to
  department-focused data marts where end users and
  applications can query it.
* Pros:
  * Single source of truth
  * Normalization = less storage
  * Easy to change data marts to support reporting changes
* Cons:
  * More joins = slower response time
  * Upfront work (higher startup cost)

![inmon top down approach](./img/03_inmon_top_down.png)

#### Kimball Bottom-up approach

* Once the data has been brought in, it is denormalized into
  a star schema
* Focus on getting from data to reporting as fast as possible
* It first organizes and defines the data of one department of
  the organization and places that data into a data mart, making
  it available for reporting. After completing one department,
  a new one is chosen, repeating the cycle.
* Various data attributes connect the data marts. The data marts
  are then integrated into a data warehouse.
* Pros:
  * Upfront development speed (lower startup cost)
  * Denormalization = user friendly
* Cons:
  * Increased ETL processing time
  * Greater possibility of duplicate data
  * Ongoing development needed

![kimbal bottom up approach](./img/04_kimbal_bottom_up.png)

## Data modeling