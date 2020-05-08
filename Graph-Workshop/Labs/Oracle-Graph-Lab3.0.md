
# Oracle Graph 

<br>
**Visualize the Graph**

GraphViz should be accessible at host:7007/ui

The principal points of entry for the GraphViz application are the query editor and the graph lists.
When you start GraphViz, the graph list will be populated with the graphs loaded in the graph server. To run queries against a graph, select that graph. The query lets you write PGQL queries that can be visualized. (PGQL is the SQL-like query language supported by GraphViz.)

Once the query is ready and the desired graph is selected, click Run to execute the query.

**Publish the oe sample graph**

````
<copy>
graph.publish(VertexProperty.ALL, EdgeProperty.ALL) ;
</copy>
````

<br>
**Scenario 1 : What products did customer 202 buy from which store(s)?**

````
<copy>
select * from oe_sample_graph 
match (c:CUSTOMERS)-[co]->(o:ORDERS)-[os]->(s:STORES), (o:ORDERS)-[e:ORDER_HAS_PRODUCT]->(p:PRODUCTS) 
where c.CUSTOMER_ID=202;
</copy>
````
 
![](./images/IMGG22.PNG)

**Add some labels to the vertices**

Click on Settings -> Then choose the Visualization tab 

![](./images/IMGG23.PNG)

Select label as the vertex label and then click OK

![](./images/IMGG23.PNG)

**The result should look like as below**

![](./images/IMGG23.PNG)


<br>
**Scenario 2 : Which customers placed orders from  store with id 1 (the Online store)? 
Show the first 100 results**

````
<copy>
Select * from oe_sample_graph 
Match (c)-[co]->(o)-[os:ORDERED_FROM_STORE]->(s)
Where s.STORE_ID=1 LIMIT 100;
</copy>
````
 
![](./images/IMGG26.PNG)

Let’s add some highlights to indicate Cancelled or Refunded orders.

Click on Settings-> Highlights-> New Highlight 

![](./images/IMGG27.PNG)

We will add two conditions that match cancelled or refunded orders.

Select Apply To Vertices (i.e. the conditions apply to Vertices)

Click on the +  sign to add a condition

Choose label = ORDERS

Again, Click + sign  to add another condition

Choose ORDER_STATUS = CANCELLED

Click the checkbox for Color (vertex color) and choose a red color from the color-picker

![](./images/IMGG28.PNG)

Scroll down and enter Cancelled as the Legend Title and then Click Add Highlight.

![](./images/IMGG29.PNG)

Repeat the above process to add one more highlight for Refunded Orders.

Select Apply To Vertices (i.e. the conditions apply to Vertices)

Click on the +  sign to add a condition

Choose label = ORDERS

Click + to add another condition

Choose ORDER_STATUS = REFUNDED

Click the checkbox for Color (vertex color) and choose a yellow color from the color-picker

Set the Legend Title to Refunded

Then Click Add Highlight.

![](./images/IMGG30.PNG)

There should now be two highlights. Click OK

![](./images/IMGG31.PNG)


The resulting viz should look like

![](./images/IMGG32.PNG)


<br>
**Scenario 3 :What products did Dale Hughes buy?**

````
<copy>
select customer, coEdge, orders, opEdge, product from oe_sample_graph match 
(customer:CUSTOMERS)-[coEdge:CUSTOMER_ORDERED]->(orders:ORDERS)-[opEdge:ORDER_HAS_PRODUCT]->(product:PRODUCTS)
where customer.FULL_NAME='Dale Hughes';
</copy>
````
 
![](./images/IMGG33.PNG)

Add highlights on edges for Order items that had Quantity > 1 and unit_Price > 25

Click on settings-> Highlights-> New Highlights -> Select Apply To Edge

Add two conditions

Click on the +  sign to add conditions

1.	One for QUANTITY > 1
2.	Another for UNIT_PRICE > 25
3.	Choose a red color for the Edge ,Click on Add Highlight and then OK.

![](./images/IMGG34.PNG)

![](./images/IMGG35.PNG)


<br>
**Scenario 4 : Which customers bought product with id 44? Shows 100 results per page**

````
<copy>
select customer, coEdge, orders, opEdge, product from oe_sample_graph match 
(orders)-[os:ORDERED_FROM_STORE]->(store:STORES),
(customer:CUSTOMERS)-[coEdge:CUSTOMER_ORDERED]->(orders:ORDERS)-[opEdge:ORDER_HAS_PRODUCT]->(product:PRODUCTS)
where store.STORE_ID=1;
</copy>
````
 
![](./images/IMGG36.PNG)

<br>
**Scenario 5 : Which customers bought product with id 44? Shows 100 results per page**

````
<copy>
select customer,opEdge, product, coEdge, orders from oe_sample_graph match 
(customer:CUSTOMERS)-[coEdge:CUSTOMER_ORDERED]->(orders:ORDERS)-[opEdge:ORDER_HAS_PRODUCT]->(product:PRODUCTS)
where product.PRODUCT_ID=44;
</copy>
````
 
![](./images/IMGG37.PNG)

<br>
**Deleting the Graph**

Once you are done using PGViz at host:7007/ui and trying some other PGQL queries then execute the following statements to delete the in-memory graph 

````
<copy>
graph.destroy();
</copy>
````

 
 
 