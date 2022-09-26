# Kimball's Dimensional Data Modeling | The Analytics Setup Guidebook
[Kimball's Dimensional Data Modeling | The Analytics Setup Guidebook](https://www.holistics.io/books/setup-analytics/kimball-s-dimensional-data-modeling/) 

 [Kimball's Dimensional Data Modeling | The Analytics Setup Guidebook](https://www.holistics.io/books/setup-analytics/kimball-s-dimensional-data-modeling/) 

 This section covers the ideas of Ralph Kimball and his peers, who developed them in the 90s, published _The Data Warehouse Toolkit_ in 1996, and through it introduced the world to dimensional data modeling.

In this section, we will present a broad-based overview of dimensional data modeling, explore why the approach has become so dominant, and then examine what bits of it we think should be brought into the modern cloud data warehousing era.

Why Kimball?
------------

There are many approaches to data modeling. We have chosen to focus on Kimball’s because we think his ideas are the most widespread, and therefore the most resonant amongst data professionals. If you hire a data analyst today, it is likely that they will be familiar with the ideas of dimensional data modeling. So you will need to have a handle on the approach to work effectively with them.

But we _should_ note that there is another approach to data modeling that is commonly mentioned in the same breath. This approach is known as Inmon data modeling, named after data warehouse pioneer Bill Inmon. Inmon’s approach was published in 1990, six years before Kimball’s. It focused on normalized schemas, instead of Kimball’s more denormalized approach.

A third data modeling approach, named Data Vault, was released in the early 2000s.

We think that many of these approaches are valuable, but that all of them are in need of updates given the rapid progress in data warehousing technology.

The Star Schema
---------------

To understand Kimball’s approach to data modeling, we should begin by talking about the star schema. The star schema is a particular way of organizing data for analytical purposes. It consists of two types of tables:

*   A fact table, which acts as the primary table for the schema. A fact table contains the primary measurements, metrics, or ‘facts’ of a business process.
*   Many dimension tables associated with the fact table. Each dimension table contains ‘dimensions’ — that is, descriptive attributes of the fact table.

These dimensional tables are said to ‘surround’ the fact table, which is where the name ‘star schema’ comes from.

![](https://cdn.holistics.io/guidebook/star-schema.png)

This is all a little abstract, so let’s go through an example to make this concrete.

Let’s say that you’re running a store, and you want to model the data from your Point of Sales system. A naive approach to this is to use your order transaction data as your fact table. You then place several dimension tables around your order table — most notably products and promotions. These three tables are linked by foreign keys — that is, each order may reference several products or promotions stored in their respective tables.

This basic star schema would thus look something like this:

![](https://cdn.holistics.io/guidebook/star-schema-exp.jpeg)

Notice how our fact table will grow very quickly over time, as we may see hundreds of orders per day. By way of comparison, our products table and promotions table would contain far fewer entries, and would be updated at a frequency much lower than the fact table.

Kimball’s Four Step Process
---------------------------

The star schema is useful because it gives us a standardized, time-tested way to think about shaping your data for analytical purposes.

The star schema is:

*   **Flexible** — it allows your data to be easily sliced and diced any which way your business users want to.
*   **Extensible** — you may evolve your star schema in response to business changes.
*   **Performant** — Kimball’s dimensional modeling approach was developed when the majority of analytical systems were run on relational database management systems (RDBMSes). The star schema is particularly performant on RDBMSes, as most queries end up being executed using the ‘star join’, which is a Cartesian product of all the dimensional tables.

But the star schema is only useful if it is easily applicable within your company. So how do you come up with a star schema for your particular business?

Kimball’s answer to that is the Four Step Process to dimensional data modeling. These four steps are as follows:

1.  **Pick a business process to model.** Kimball’s approach begins with a business process, since ultimately, business users would want to ask questions about processes. This stands in contrast to earlier modeling methodologies, like Bill Inmon’s, which started with the business entities in mind (e.g. the customer model, product model, etc).
2.  **Decide on the grain**. The grain here means the level of data to store as the primary fact table. It should be the most _atomic_ level possible — that is, a level of data that cannot be split further. For instance, in our Point of Sales example earlier, the grain should actually be the _line items_ inside each order, instead of the order itself. This is because in the future, business users may want to ask questions like “what are the products that sold the best during the day in our stores?” — and you would need to drop down to the line-item level in order to query for that question effectively. In Kimball’s day, if you had modeled your data at the order level, such a question would take a huge amount of work to get at the data, because you would run the query on slow database systems. You might even need to do ETL again, if the data is not currently in a queryable form in your warehouse! So it is best to model at the lowest level possible from the beginning.  
    
3.  **Chose the dimensions that apply to each fact table row.** This is usually quite easy to answer if you have ‘picked the grain’ properly. Dimensions fall out of the question “how do businesspeople describe the data that results from the business process?” You will decorate fact tables with a robust set of dimensions representing all possible descriptions.
4.  **Identify the numeric facts that will populate each fact table row.** The numeric data in the fact table falls out of the question “what are we answering?” Business people will ask certain obvious business questions (e.g. what’s the average profit margin per product category?), and so you will need to decide on what are the most important numeric measures to store at the fact table layer, in order to be recombined later to answer their queries. Facts should be true to the grain defined in step 2; if a fact belongs to a different grain, it should live in a separate fact table.

In the case of a retail POS, if we go through the four steps, above, we would model line items, and would end up with something like this:

![](https://cdn.holistics.io/guidebook/retail-sales-schema.jpeg)

Notice how the dimension tables are oriented out from around the fact table. Note also how fact tables consist of foreign keys to the dimensional tables, and also how ‘numeric facts’ — fields that can be aggregated for business metric purposes — are carefully chosen at the line item fact table.

Now notice that we have a _date dimension_ as well:

![](https://cdn.holistics.io/guidebook/retail-sales-schema-2.jpeg)

This might be surprising to you. Why would you have something like a date dimension, of all things? The answer is to make things easier to query for the business user. Business users might like to query in terms of fiscal year, special holidays, or selling seasons like Thanksgiving and Christmas. Since these concepts aren’t captured in the date field of an RDBMS system, we need to model date as an explicit dimension.

This captures a core philosophy of Kimball’s approach, which is to **do the hard work now, to make it easy to query later**.

This short example gives you all the flavor of dimensional data modeling. We can see that:

1.  The fact and dimension tables give us a standardized way to think about shaping your analytical data. This makes your work as a data analyst a lot easier, since you are guided by a certain structure.
2.  Kimball’s four steps can be applied to _any_ business process (and he proves this, because _every chapter in_ The Data Warehouse Toolkit _covers a different business process!_)
3.  The star schema that falls out of this results in flexibility, extensibility, and performance.
4.  The star schema works well given the performance constraints that Kimball worked with. Remember that memory was relatively expensive during Kimball’s time, and that analytical queries were either run on top of RDBMSes, or exported into OLAP cubes. Both approaches benefited from a well-structured dimensional data model.

Why Was Kimball’s Approach Needed?
----------------------------------

Before we discuss if these techniques are applicable today, we must ask: why were these data modeling techniques introduced in the first place? Answering this question helps us because we may now evaluate if the underlying reasons have changed.

The dimensional data modeling approach gained traction when it was first introduced in the 90s because:

1.  **It gave us speed to business value.** Back in the day, data warehouse projects were costly affairs, and needed to show business value as quickly as possible. Data warehouse designers before the Kimball era would often come up with normalized schemas. This made query writing very complicated, and made it more difficult for business intelligence teams to deliver value to the business quickly and reliably. Kimball was amongst the first to formally realize that denormalized data worked better for analytical workloads compared to normalized data. His notion of the star schema, alongside the ‘four steps’ we discussed earlier in this section, turned his approach into a repeatable and easily applicable process.
2.  **Performance reasons.** As we’ve mentioned earlier, in Kimball’s time, the majority of analytical workloads were still run on RDBMSes (as Kimball asserts himself, in _The Data Warehouse Toolkit_). Scattered throughout the book are performance considerations you needed to keep in mind, even as they expanded on variations of schema design — chief amongst them is the idea that star schemas allowed RDBMSes to perform highly efficient ‘star joins’. In a sentence: dimensional modeling had _very real_ _benefits_ when it came to running business analysis — so large, in fact, that you simply couldn’t ignore it. Many of these benefits applied even when people were exporting data out from data warehouses to run in more efficient data structures such as OLAP cubes.

We think that Kimball’s ideas are so useful and so influential that we would be unwise to ignore them today. But now that we’ve examined the reasons that it rose in prominence in the first place, we must ask: how relevant are these ideas in an age of cloud-first, incredibly powerful data warehouses?

Kimball-Style Data Modeling, Then And Now
-----------------------------------------

The biggest thing that has changed today is the difference in costs between data labor versus data infrastructure.

Kimball data modeling demanded that you:

*   Spent time up front designing the schema
*   Spent time building and maintaining data pipelines to execute such schemas (using ETL tools, for the most part)
*   Keep a dedicated team around that is trained in Kimball’s methodologies, so that you may evaluate, extend, and modify existing star schemas in response to business process changes.

When data infrastructure was underpowered and expensive, this investment made sense. Today, cloud data warehouses are many times more powerful than old data warehouses, and come at a fraction of the cost.

Perhaps we can make that more concrete. In _The Data Warehouse Toolkit_, Kimball described a typical data warehouse implementation project with the following illustration:

![](https://cdn.holistics.io/guidebook/typical-dw-implementation-project.jpeg)

A typical project would go like this: you would write ETL to consolidate data sources from different source systems, accumulate data into a staging area, then use an ETL tool (again!) to model data into a data presentation area. This data presentation area consists of multiple data marts. In turn, these ‘marts’ may be implemented on top of RDBMSes, or on top of an OLAP cube, but the point is that the marts must contain dimensionally modeled data, and that data must be _[conformed](https://www.kimballgroup.com/data-warehouse-business-intelligence-resources/kimball-techniques/dimensional-modeling-techniques/conformed-dimension/)_ across the entire data warehouse project.

Finally, those data marts are consumed by data presentation tools.

You will notice that this setup is vastly more complicated than our approach. Why is this the case?

Again, the answer lies in the technology that was available at the time. Databases were slow, computer storage was expensive, and BI tools needed to run on top of OLAP cubes in order to be fast. This demanded that the data warehouse project be composed of a number of separate data processing steps.

Today, things are much better. Our approach assumes that you can do away with many elements of Kimball’s approach.

We shall give two examples of this, before we generalize to a handful of principles that you may apply to your own practice.

### Example 1: Inventory Management

In _The Data Warehouse Toolkit_, Ralph Kimball describes how keeping track of inventory movements is a common business activity for many types of businesses. He also notes that a fact table consisting of _every single inventory move is too large to do good analysis on._

Therefore, he dedicates an entire chapter to discuss various techniques to get around this problem. The main solution Kimball proposes is to use ETL tools to create ‘snapshot’ fact tables, that are basically aggregated inventory moves for a certain time period. This snapshotting action is meant to occur on a regular basis.

Kimball then demonstrates that data analysis can happen using the aggregated snapshot tables, and only go down to the inventory fact table for a minority of queries. This helps the business user because running such queries on the full inventory table is often a performance nightmare.

Today, modern cloud data warehouses have a number of properties to make this ‘snapshotting’ less of a hard requirement:

1.  Modern cloud data warehouses are usually backed by a columnar data architecture. These columnar data stores are able to chew through _millions_ of rows in seconds. The upshot here is that you can throw out the entire chapter on snapshot techniques and still get relatively good results.
2.  Nearly all modern cloud data warehouses run on massively parallel processing (MPP) architectures, meaning that the data warehouse can dynamically spin up or down as many servers as is required to run your query.
3.  Finally, cloud data warehouses charge by usage, so you pay a low upfront cost, and only pay for what you use.

These three requirements mean that it is often more expensive to hire, train and retain a data engineering team necessary to maintain such complex snapshotting workflows. It is thus often a better idea to run all such processes directly on inventory data within a modern columnar data warehouse.

(Yes, we can hear you saying “but snapshotting is still a best practice!” — the point here is that it’s now an _optional_ one, not a hard must.)

### Example 2: Slowly Changing Dimensions

What happens if the dimensions in your dimension tables change over time? Say, for instance, that you have a product in the education department:

![](https://cdn.holistics.io/guidebook/kimball-exp-1.png)

And you want to change IntelliKidz 1.0’s department to ‘Strategy’.

![](https://cdn.holistics.io/guidebook/kimball-exp-2.png)

The simplest strategy you may adopt is what Kimball calls a ‘Type 1’ response: you update the dimension naively. This is what has happened above. The good news is that this response is simple. The bad news is that updating your dimension tables this way will mess up your old reports.

For instance, if management were to run the old revenue reports again, the same queries that were used to calculate revenue attributed to the Education department would now return different results — because IntelliKidz 1.0 is now registered under a different department! So the question becomes: how do you register a change in one or more of your dimensions, while still retaining the report data?

This is known as the ‘slowly changing dimension’ problem, or ‘dealing with SCDs’.

Kimball proposed three solutions:

The first, ‘Type 1’, is to update the dimension column naively. This approach has problems, as we’ve just seen.

The second, ‘Type 2’, is to add a new row to your product table, with a new product key. This looks as follows:

![](https://cdn.holistics.io/guidebook/kimball-exp-3.png)

With this approach, all new orders in the fact table will refer to the product key 25984, not 12345. This allows old reports to return the same numbers.

The final approach, ‘Type 3’, is to add a new column to the dimension table to capture the previous department. This setup supports the ability to view an ‘alternate reality’ of the same data. The setup thus looks like this:

![](https://cdn.holistics.io/guidebook/kimball-exp-4.png)

Kimball’s three approaches require some effort when executing. As a side effect, such approaches make querying and writing reports rather complicated affairs.

So how do you handle SCDs today?

In a [2018 talk](https://www.youtube.com/watch?v=4Spo2QRTz1k&t=989s) at Data Council, senior Lyft data engineer Maxime Beauchemin describes an approach that is currently used in Facebook, Airbnb, and Lyft.

The approach is simple: many modern data warehouses support a table partitioning feature. Beauchemin’s idea is to use an ETL tool to create and copy new table partitions as a ‘snapshot’ of _all_ the dimensional data, on a daily or weekly basis.

This approach has a number of benefits:

1.  As Beauchemin puts it: “Compute is cheap. Storage is cheap. Engineering time is expensive.” This approach is as pure a tradeoff between computational resources and engineering time.
2.  Dimensional data is small and simple when compared to fact data. This means that even a couple thousand rows, snapshotted going back ten years, is a drop in the bucket for modern data warehouses.
3.  Finally, snapshots give analysts an easy mental model to reason with, compared to the queries that you might have to write for a Type 2 or Type 3 response.

As an example of the third benefit, Beauchemin presents a sample query to demonstrate the simplicity of the mental model required for this approach:

```
--- With current attribute
select * 
FROM fact a 
JOIN dimension b ON
    a.dim_id = b.dim_id AND
    date_partition = `{{ latest_partition('dimension') }}`

--- With historical attribute 
select * 
FROM fact a 
JOIN dimension b ON
    a.dim_id = b.dim_id AND
    a.date_partition = b.date_partition

```

Really simple stuff.

The key insight here is that _storage is really cheap today_. When storage is cheap, you can get away with ‘silly’ things like partitioning every dimension table every day, in order to get a full history of slowly changing dimensions.

As Beauchemin mentions at the end of his talk: “the next time someone talks to you about SCD, you can show them this approach and tell them it’s solved.”

Applying Kimball Style Dimensional Modeling to the Data Infrastructure of Today
-------------------------------------------------------------------------------

So how do we blend traditional Kimball-style dimensional modeling with modern techniques?

We’ve built Holistics with a focus on data modeling, so naturally we think there _is_ value to the approach. Here are some ideas from our practice, that we think can apply generally to your work in analytics:

### **Kimball-style dimensional modeling is effective**

Let’s give credit where credit is due: Kimball’s ideas around the star schema, his approach of using denormalized data, and the notion of dimension and fact tables are powerful, time-tested ways to model data for analytical workloads. We use it internally at Holistics, and we recommend you do the same.

We think that the question isn’t: ‘is Kimball relevant today?’ It’s clear to us that the approach remains useful. The question we think _is_ worth asking is: ‘is it possible to get the benefits of dimensional modeling without _all_ the busy work associated with it?’

And we think the answer to that is an unambiguous yes.

### Model As And When You Need To

We think that the biggest benefit of having gobsmacking amounts of raw computing power today is the fact that such power allows us increased flexibility with our modeling practices.

By this we mean that you should _model when you have to._

Start with generating reports from the raw data tables from your source systems — especially if the reports aren’t too difficult to create, or the queries not too difficult to write. If they are, model your tables to match the business metrics that are most important to your users — without too much thought for future flexibility.

Then, when reporting requirements become more painful to satisfy — and _only when they become painful to satisfy —_ you may redo your models in a more formal dimensional modeling manner.

Why does this approach work? It works because transformations are comparatively easy when done within the same data warehouse. It is here that the power of the ELT paradigm truly shows itself. When you have everything stored in a modern data warehouse, you are able to change up your modeling approach as and when you wish.

This seems like a ridiculous statement to make — and can be! — especially if you read it within the context where Kimball originally developed his ideas. _The Data Warehouse Toolkit_ was written at a time when one had to create new ETL pipelines in order to change the shape of one’s data models. This was expensive and time consuming. This is not the case with our approach: because we recommend that you centralize your raw data within a data warehouse first, you are able to transform them into new tables within the same warehouse, using the power of that warehouse_._

This is even easier when coupled with tools that are designed for this paradigm.

What are some of these tools? Well, we’ve introduced these tools in the previous section of the book. We called these tools ‘data modeling layer tools’, and they are things like Holistics, dbt, and Looker.

The common characteristic among these tools is that they provide helpful structure and administrative assistance when creating, updating, and maintaining new data models. For instance, with Holistics, you can visualize the lineage of your models. With dbt and Looker, you can track changes to your models over time. Most tools in this segment allow you to do incremental updating of your models.

These tools then generate the SQL required to create new data models and persist them into new tables within the same warehouse. Note how there is no need to request data engineering to get involved to set up (and maintain!) external transformation pipelines. Everything happens in one tool, leveraging the power of the underlying data warehouse.

The upshot: it is no longer necessary to treat data modeling as a big, momentous undertaking to be done at the start of a data warehousing project. With ‘data modeling layer tools’, you no longer need data engineering to get involved — you may simply give the task of modeling to anyone on your team with SQL experience. So: do it ‘just-in-time’, when you are sure you’re going to need it.

### Use Technology To Replace Labor Whenever Possible

A more general principle is to use technology to replace labor whenever possible.

We have given you two examples of this: inventory modeling, and dealing with slowly changing dimensions. In both, Kimball’s approach demanded a level of manual engineering. The contemporary approach is to simply rely on the power of modern data infrastructure to render such manual activities irrelevant.

With inventory modeling, we argued that the power of MPP columnar data warehouses made it possible to skip aggregation tables … unless they were absolutely necessary. Your usage should drive your modeling requirements, and not the other way around.

With SCDs, we presented an approach that has been adopted at some of the largest tech companies: that is, recognize that storage is incredibly cheap today, and use table partitions to snapshot dimensional data over time. This sidesteps the need to implement one of the three responses Kimball details in his approach.

In both cases, the idea is to **critically evaluate the balance between computing cost and labor cost**. Many of Kimball’s techniques should not be adopted if you can find some way to sidestep it using contemporary cloud data warehousing functionality.

Data architects trained in the old paradigm are likely to balk at this approach. They look at potential cloud DW costs, and gasp at the extra thousands of dollars you might have to pay if you push the heavy-lifting to the data warehouse. But remember this: it is usually far more costly to hire an extra data engineer than it is to pay for the marginal cost of DW functionality. Pushing BigQuery to aggregate terabytes of data might cost you an extra 1000 dollars of query time a month. But hiring an extra data engineer to set up and maintain a pipeline for you is going to cost many times more than that, especially if you include the full cost of employee benefits.

Conclusion
----------

Think holistically about your data infrastructure. The best companies we work with do more with fewer people. They use the power of their data warehouses to increase the impact of the people they have, and choose to hire data analysts (who create reusable models) over data engineers (who create extra infra).

You should consider doing the same.