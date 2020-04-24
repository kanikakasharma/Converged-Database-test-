
# Oracle Graph 

**Scenario**

1.  Which products did **customer 40202** buy from which store(s)?

 ````
 <copy>
 select * from graph match (c:Customers)-[co]->(o:Orders)-[os]->(s:Stores), (o:Orders)-[e:Order_Has_Product]->(p:Products) where id(c)=40202
 </copy>
 ````
  

![](./images/IMG1.PNG " ") 

**SQL Query**

````
<copy>   
select count(distinct vid) from graphvt$ ;
</copy>
````    
    COUNT(DISTINCTVID)
    -------------------
       2410
 

````
<copy>
select count(distinct eid) from graphge$;
</copy>
````

    COUNT(DISTINCTVID)
    -------------------
       11632

    
   

 **PGQL  query:**
 
 
````
<copy>    
opg-jshell-rdbms> query.accept(“select count(v) from graph match(v)“);
    </copy>
````

    count(v) 
    --------
    2410
  
 
````
<copy>
opg-jshell-rdbms> query.accept(“select count(e) from graph match ()-[e]->()“);
</copy>
````

    count(e) 
    --------
    11632
   
   
**Check connection between every nodes like (Customers, Products, Orders):**

````
<copy>  
opg-jshell-rdbms> query.accept(“select distinct label(e) from graph match ()-[e]->(m)“) ;
</copy>
````

    +--------------------+
    | label(e)      |
    +--------------------+
    | Order_Has_Product |
    | Store_Got_Order  |
    | Ordered_From_Store |
    | Customer_Ordered  |
    | Ordered_By     |
    +--------------------+


**What are the nodes here ?**

````

   <copy>
opg-jshell-rdbms> query.accept(“select distinct label(v) from graph match (v)“) ; 
    </copy>
````

    +-----------+
    | label(v) |
    +-----------+
    | Orders  |
    | Customers |
    | Stores  |
    | Products |
    +-----------+



