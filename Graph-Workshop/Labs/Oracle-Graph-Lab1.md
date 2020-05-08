
# Oracle Graph 

<br>

**Scenario 1 : Which stores did customer with id 202 order from?**

 ````
 <copy>
 query.accept("select s.STORE_NAME from oe_sample_graph match (c:CUSTOMERS)->(o:ORDERS)->(s:STORES) where c.CUSTOMER_ID=202");
 </copy>
 ````
  
![](./images/IMGG11.PNG " ") 

<br>
**Scenario 2 : What did Dale Hughes buy?**

````
<copy>
query.accept(
"select o.ORDER_STATUS, op.QUANTITY, p.UNIT_PRICE, p.PRODUCT_NAME from oe_sample_graph match (c)-[co]->(o:ORDERS)-[op]->(p:PRODUCTS) where c.FULL_NAME='Dale Hughes'");
</copy>
````
 
![](./images/IMGG12.PNG)

<br>

**Scenario 3 : What did people buy from the Online Store. Return first 50 results.**

 ````
 <copy>
 query.accept(
"select c.FULL_NAME, p.PRODUCT_NAME from oe_sample_graph match (o)-[os:ORDERED_FROM_STORE]->(s:STORES),(c)-[co]->(o:ORDERS)-[op]->(p:PRODUCTS) where s.STORE_ID=1 limit 50");
 </copy>
 ````
  
![](./images/IMGG13.PNG " ") 


<br>
**Scenario 4 : Who bought how much of product  with id 19**

````
<copy>
query.accept("select c.FULL_NAME, op.QUANTITY from oe_sample_graph match (c)-[co]->(o:ORDERS)-[op]->(p:PRODUCTS) where p.PRODUCT_ID=19 order by op.QUANTITY desc");
</copy>
````
 
![](./images/IMGG14.PNG)


<br>
**Scenario 5 : Which customers bought products that customer 202 bought? Return the first 10 results  that had the most products in common with 202**

````
<copy>
var qStr = 
"select c1.FULL_NAME " + 
"FROM oe_sample_graph " + 
"MATCH (c:CUSTOMERS)->(:ORDERS)-[:ORDER_HAS_PRODUCT]->(p:PRODUCTS)," +
"(c1:CUSTOMERS)->(:ORDERS)-[:ORDER_HAS_PRODUCT]->(p:PRODUCTS) " +
"WHERE c.CUSTOMER_ID=202 " + 
"AND c.CUSTOMER_ID <> c1.CUSTOMER_ID " +
"GROUP BY c1 " + 
"ORDER BY count(DISTINCT p) DESC " + 
"LIMIT 10";

query.accept(qStr);
</copy>
````
 
![](./images/IMGG15.PNG)


