# Goods-To-Consumer-Analysis
Using a Google BigQuery, fictitious ecommerce database, I used SQL and DAX to build a Power BI dashboard for analysis.

## Data Source

[Google BigQuery](https://console.cloud.google.com/) Google Open Datasets, "thelook_ecommerce"

## Project Overview

In this project, I pretended I was given a task by the Vice President of Logistsics to discover any and all major issues with the back end of the clothing and textile ecommerce company's supply chain; this company is named "TheLook." This led to a Goods-To-Consumer-Type dashboard and analysis. The Vice President wanted a way to quickly injest all order and shipping data in a simple, yet holistic, manner in real time and at the national level. (Knowing this is a Google-generated fictitious database, I was bound to find some things that were odd.)

## Database Structure

The "thelook_ecommerce" database in the Google BigQuery open datasets environment consists of 6 tables:
- **distribution_centers (dimension)** - *as the name describes*
- **events (fact)** - *web data as it applies to each users' purchase*
- **inventory_items (dimension)** - *items contained in inventory*
- **order_items (fact)** - *the items within each order*
- **orders (fact)** - *orders made and the orders' attributes*
- **products (dimension)** - *products and their attributes*
- **users (dimension)** - *customers*

### Rough Database Diagram

![databasediagram](https://user-images.githubusercontent.com/103079066/232253675-0750967c-25ab-42a2-9741-1572b040691a.jpg)

## Methodology

With the VP being a very busy person, there would be very limited time to sift through many visuals on a report page. With that in mind, I opted for the multi-page approach where each page had a specific purpose and a limited amount of visuals.

I used SQL as the method to extract the data from the database. I chose not to use a live connection; rather, I used the option to make a copy of the data in the Power BI environment. Using this method increases the performance of the dashboard and removes limits on building measures and calculated columns. If I were to publish this dashboard, I would set the dashboard to refresh every night at midnight; this way, the data would be current to the most recently completed business day.

## Data Model

### Source Query and Power Query

1. The original SQL query extracted data for orders, where they came from, their current status, which distribution center they came from, and the items contained in each order.

![SourceQuery](https://user-images.githubusercontent.com/103079066/232253932-31afdcf8-afd5-42e0-a1bf-c3ca0ef8d77b.png)

2. Next, I created a copy of this table using Power Query, deleted duplicate order IDs, and deleted product columns. This would be the table I would actually use for the report visuals and measures. My purpose for doing this was to create a clean table that focused exclusively on distribution center performance; however, I still had an original table with product information for future scalability, if desired.
3. In order to maintain a scalable model using an auto calendar table, I changed each DATETIME column to a DATE column. Power BI's time intelligence feature will lock up if you attempt to utilize it with DATETIME columns. It is not designed for data at such a granular scale, nor is it necessary.
4. I created a calendar table with **CALENDARAUTO()** using the "create table" option. I joined this calendar table to our duplicate table for clean time series visuals and analysis. I set the fiscal year to end on 6/30 and begin on 7/1.

### Calculated Columns

The second part of the data modeling process required me to design some flags for unique order ID KPIs to measure performance.

1. **Delayed Shipping Flag** - *any order that took longer than two days to ship is considered delayed shipping, but I also made sure to filter out blank created_at and shipped_at cells to stay within the scope of the measure*

![DelayedShippingFlag](https://user-images.githubusercontent.com/103079066/232577675-459e95c9-61e2-49f5-bd80-e51ffe1bf31e.png)

2. **Failed to Ship Flag** - *any order that has been stuck in the processing stage for longer than 7 days*

![FailedToShipFlag](https://user-images.githubusercontent.com/103079066/232577877-493810c4-a9b3-4bf6-98c8-9086398e7a54.png)

3. **Unshipped Order Flag** - *any order that is not yet late past the 2-day flag, but is waiting to be shipped*

![UnshippedOrderFlag](https://user-images.githubusercontent.com/103079066/232579395-abaef018-79e7-45d7-984a-c50d9361810a.png)

### Measures

1. **Delayed Shipping Rate** - *the percent rate at which orders are considered "delayed" using the flag custom column*

![DelayedShippingRate](https://user-images.githubusercontent.com/103079066/232580462-5e339bb8-830f-43cf-ad17-e8a8bd23fea3.png)

2. **Delayed Shipping Target** - *the same delayed shipping rate but calucalated for the same period last year; for use with a KPI card*

![DelayedShippingTarget](https://user-images.githubusercontent.com/103079066/232580678-9a873655-a31e-44eb-aa6b-379a41eba1d0.png)

3. **Order Failure Rate** - *the percent rate at which orders are considered "failed" using the flag custom column*

![OrderFailureRate](https://user-images.githubusercontent.com/103079066/232580896-2c5ca793-1e35-4c76-ab79-609e8eeb84ff.png)

4. **Order Failure Target** - *the same failed shipping rate but calculated for the same period last year; for use with a KPI card*

![OrderFailureTarget](https://user-images.githubusercontent.com/103079066/232581126-330cfc9e-624d-48a3-8d7c-9e6b69fdb75f.png)

## Report Construction

### Pages

1. **Overview** - *an overall dashboard with a high-level status pipeline, visualized as a heat map matrix; and an order failure stacked bar chart, to see how many orders consistent of failures--all filterable with a date slider*
-*clicking a distribution center in the matrix will update the overview cards at the top for that specific warehouse*

<img width="658" alt="image" src="https://user-images.githubusercontent.com/103079066/232587182-caf1c75d-87e3-41c5-83a8-1b47498abb5b.png">

2. **Delayed Shipping Over Time** - *a detailed view and YoY KPI of how each distribution center performs with shipping delays over time; can be isolated to view a single distribution center and specific year*

<img width="658" alt="image" src="https://user-images.githubusercontent.com/103079066/232587574-9a550187-12f2-42ee-9302-5c91da816904.png">

3. **Failed Shipping Over Time** - *a detailed view and YoY KPI of how each distribution center performs with shipping failures over time; can be isolated to view a single distribution center and specific year*

<img width="658" alt="image" src="https://user-images.githubusercontent.com/103079066/232587777-bfc96bbf-c940-4810-a544-0fdc645251af.png">

4. **Shipping Decomp Analysis** - *a supplementary tree map to view the flow of goods to consumers and the amounts of items contained in each shipping status*

<img width="317" alt="image" src="https://user-images.githubusercontent.com/103079066/232587975-18122def-d491-4965-ba46-463ec5f423a1.png">

## Analysis

***Because this is a refreshable dashboard, the current analysis documented is as of 4/15/2023.***

As mentioned earlier in the documentation, the data presented was bound to have oddities. With the VP of Logistics tasking me to find major areas or concern as well as a monitoring process, I quickly developed two KPIs.

1. There is an order failure rate of 33.38%, nationally. I don't have the data to dive into this issue any further, but this amount of orders placed that never shipped is a major, major problem. Any national ecommerce business cannot survive for long with logistics failures of this magnitude.
2. Of the orders that did ship, 8.58% took longer than two days to ship. This is not good but a more manageable area to tackle.
- The Los Angeles, Philadephia, and Mobile distribution centers seem to be correcting delayed shipping errors over time.
3. The Los Angeles distribution center has maintained a decreasing failure and delayed shipping rate (after 2020) since the start up of the company. This distribution center is the only distribution center to maintain decreasing rates for both KPIs.

<img width="634" alt="image" src="https://user-images.githubusercontent.com/103079066/232591468-5d93624f-7f67-42af-8e74-8b1af91d12a2.png">
<img width="635" alt="image" src="https://user-images.githubusercontent.com/103079066/232591544-29d9f948-2438-4290-bc2b-d60210d02321.png">

4. Most distribution centers are receiving returns close to 10% of the time per each fiscal year.

<img width="653" alt="image" src="https://user-images.githubusercontent.com/103079066/232600151-4cea1ba2-047f-44f1-a994-57b0a003e79b.png">

## Recommendations and Conclusion

The very serious problem areas discovered in this report should prompt at least two more targeted analyses.

1. We need to start collecting data on warehouse staffing and design. We don't have the data available in the current "thelook_ecommerce" database to investigate further. With the very poor rates on order failure and delays being on a national scale, its guaranteed that the problems are occuring in the logistics model.
2. We need to collect data on the software and/or ordering system used within the company. We have access to the "events" table, which could be joined to our "orders" table. Doing an analysis on web events could help use find if any specific browsers or areas contribute to order failure.
- This would be out of scope of the current dashboard, so that will have to be a separate analysis.
3. Visit the Los Angeles distribution center to meet with leadership. This is a great location to benchmark how they are correcting shipping errors.
4. Collect data by way of quarterly surveys or day-of return reasons to diagnose the magnitute of returns.

Overall, TheLook has a few very large issues if they are to recover from this logistical nightmare that has occured over the past 5 years. Immediately tasking data analyst teams on the 4 projects above is vital to the survival of the company.

**I encourage anyone who has read this project and analysis to investigate possible causes of TheLook's goods-to-consumer nightmare.**

**Thank you for reading!**
