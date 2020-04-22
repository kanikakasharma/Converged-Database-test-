
# Oracle Graph 

## Lab1

<br>

**Scenario**

<br>
<br>

 **Which products did customer 40202 buy from which store(s)?**
</br>
</br>
 


 
   SQL> select * from graph match (c:Customers)-[co]->(o:Orders)-[os]->(s:Stores), (o:Orders)-[e:Order_Has_Product]->(p:Products) where id(c)=40202
  
 
    
    
![](./images/IMG1.PNG)

**SQL Query**
   
    SQL>select count(distinct vid) from graphvt$ ;
    
    COUNT(DISTINCTVID)
    -------------------
       2410
 


    SQL> select count(distinct eid) from graphge$;

    COUNT(DISTINCTVID)
    -------------------
       11632

    
   

 **PGQL  query:**
 
 
    
    opg-jshell-rdbms> query.accept(“select count(v) from graph match(v)“);
    
    count(v) 
    --------
    2410
  
 
   ````
   opg-jshell-rdbms> query.accept(“select count(e) from graph match ()-[e]->()“);
 count(e) 
 --------
 11632
    ````
   
**Check connection between every nodes like (Customers, Products, Orders):**

  
    opg-jshell-rdbms> query.accept(“select distinct label(e) from graph match ()-[e]->(m)“) ;
    
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

   
    opg-jshell-rdbms> query.accept(“select distinct label(v) from graph match (v)“) ; 
    
    +-----------+
    | label(v) |
    +-----------+
    | Orders  |
    | Customers |
    | Stores  |
    | Products |
    +-----------+



