# Goods-To-Consumer-Analysis
Using a Google BigQuery, fictitious ecommerce database, I used SQL and DAX to build a Power BI dashboard for analysis.

## Data Source

[Google BigQuery](https://console.cloud.google.com/) Google Open Datasets, "thelook_ecommerce"

## Project Overview

In this project, I pretended I was given a task by the Vice President of Logistsics to discover any and all major issues with the back end of the ecommerce company's, named "TheLook," supply chain. This led to a Goods-To-Consumer-Type dashboard and analysis. The Vice President wanted a way to quickly injest large amounts of data in a simple, yet holistic, manner in real time.(Knowing this is a Google-generated fictitious database, I was bound to find some things that were odd.)

## Data Structure

The "thelook_ecommerce" database in the Google BigQuery open datasets environment consists of 6 tables:
- distribution_centers (dimension) - *as the name describes*
- events (fact) - *web data as it applies to each users' purchase*
- inventory_items (dimension) - *items contained in inventory*
- order_items (fact) - *the items within each order*
- orders (fact) - *orders made and the orders' attributes*
- products (dimension) - *products and their attributes*
- users (dimension) - *customers*

### Rough Database Diagram

![databasediagram](https://user-images.githubusercontent.com/103079066/232253675-0750967c-25ab-42a2-9741-1572b040691a.jpg)

## Methodology

With the VP being a very busy person, there would be very limited time to sift through many visuals on a report page. With that in mind, I opted for the multi-page approach where each page had a specific purpose and a limited amount of visuals.

I used SQL as the method to extract the data from the database. I chose not to use a live connection; rather, I used the option to make a copy of the data in the Power BI environment. Using this method increases the performance of the dashboard and removes limits on building measures and calculated columns. If I were to publish this dashboard, I would set the dashboard to refresh every night at midnight; this way, the data would be current to the most recently completed business day.

### Source Query:

![SourceQuery](https://user-images.githubusercontent.com/103079066/232253932-31afdcf8-afd5-42e0-a1bf-c3ca0ef8d77b.png)
