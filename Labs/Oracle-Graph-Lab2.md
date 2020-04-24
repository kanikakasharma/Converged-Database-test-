
# Oracle Graph 

**Scenario**

2. First **100 results** of customers and orders from store **30001?**

 
![](./images/IMG2.PNG)

 
 **Steps to install Oracle Graph  Server:**

    ````
    <copy> 
    cd /home/opc
    chown -R root:root oracle-graph-20.1.0.x86_64.rpm
    chmod 777 oracle-graph-20.1.0.x86_64.rpm
    </copy>
    ````
    
    

    
    <copy>
    rpm -i oracle-graph-20.1.0.x86_64.rpm
    </copy>
    
    <copy>
    usermod -a -G oraclegraph oracle
    useradd graphuser
    groupadd oraclegraph
    usermod -a -G oraclegraph oracle
    </copy>
    
    <copy>
    ls -ld /etc/oracle/graph/
    cd /etc/oracle
    </copy>
    
     ````
    <copy>
    chown -R oracle:oinstall graph/
    </copy>
     ````

    locate server.conf
    cd /etc/oracle/graph/
    usermod -a -G oraclegraph graphuser
    cp server.conf server.conf_old
    vi server.conf

    [root@database-upg graph]# more server.conf
    {
    "port": 7007,
    "enable_tls": false,  changed from TRUE
    "enable_client_authentication": true,
    "working_dir": "/opt/oracle/graph/pgx/tmp_data"
    }
    [root@database-upg graph]#

    cd /opt/oracle/graph/pgx/tmp_data
  
    cd /opt/oracle/graph/pgx/bin
    
   ![](./images/IMG3.PNG) 
   
   ![](./images/IMG4.PNG) 
   
   
   **Steps to install Oracle Graph Client:**
   
    cd /home/opc
    ls -alrt
    chown -R oracle:oinstall oracle-graph-client-20.1.0.zip
    cp oracle-graph-client-20.1.0.zip /u01
    chown -R oracle:oinstall jdk-11.0.5_linux-x64_bin.tar.gz
    cp jdk-11.0.5_linux-x64_bin.tar.gz /u01
    
    
    [oracle@database-upg bin]$ ls
    opg.bat     opg-hbase.bat     opg-hbase-jshell  opg-nosql.bat     opg-nosql-jshell  opg-rdbms-groovy
    opg-groovy  opg-hbase-groovy  opg-jshell        opg-nosql-groovy  opg-rdbms.bat     opg-rdbms-jshell
    [oracle@database-upg bin]$ pwd
    /u01/graphclient/oracle-graph-client-20.1.0/bin
    [oracle@database-upg bin]$
    
    
    /u01/app/oracle/product/19.3.0/dbhome_2/jdk/
    [oracle@database-upg bin]$ cd /u01/jdk-11.0.5/
    [oracle@database-upg jdk-11.0.5]$ export JAVA_HOME=/u01/jdk-11.0.5/
    
    
    [oracle@database-upg jdk-11.0.5]$ echo $JAVA_HOME
    /u01/jdk-11.0.5/
    [oracle@database-upg jdk-11.0.5]$ cd /u01/graphclient/oracle-graph-client-20.1.0/bin
    
    
    [oracle@database-upg bin]$ ./opg-rdbms-jshell --base_url http://129.213.155.161:7007
    [WARNING] CLASSPATH environment will be prepended to PGX classpath. If this is not intended, do 'unset CLASSPATH' and restart.
    For an introduction type: /help intro
    Oracle Graph Server Shell 20.1.0
    PGX server version: 19.4.0 type: SM
    PGX server API version: 3.6.0
    PGQL version: 1.2
    Variables instance, session, and analyst ready to use.
    
     
    opg-jshell-rdbms> var jdbcUrl = "jdbc:oracle:thin:@129.213.155.161:1530/spagrapdb";
    jdbcUrl ==> "jdbc:oracle:thin:@129.213.155.161:1530/spagrapdb"
    opg-jshell-rdbms> var user = "graphuser";
    user ==> "graphuser"
    opg-jshell-rdbms> var pass = "graphuser";
    pass ==> "graphuser"
    opg-jshell-rdbms> var conn = DriverManager.getConnection(jdbcUrl, user, pass) ;
    conn ==> oracle.jdbc.driver.T4CConnection@20c9a0c8
    opg-jshell-rdbms> conn.setAutoCommit(false);
    opg-jshell-rdbms> var pgql = PgqlConnection.getConnection(conn);
    pgql ==> oracle.pg.rdbms.pgql.PgqlConnection@49fb693d
    opg-jshell-rdbms> Consumer<String> query = q -> { try(var s = pgql.prepareStatement(q)) { s.execute(); s.getResultSet().print(); } catch(Exception e) { throw new RuntimeException(e); } }
    query ==> $Lambda$585/0x0000000800636840@5392aa2b
    opg-jshell-rdbms> query.accept("select distinct label(e) from graph match ()-[e]->(m)");
    
    +--------------------+
    | label(e)           |
    +--------------------+
    | Order_Has_Product  |
    | Store_Got_Order    |
    | Ordered_From_Store |
    | Customer_Ordered   |
    | Ordered_By         |
    +--------------------+
    
    
    opg-jshell-rdbms> query.accept("select distinct label(v) from graph match (v)") ;
    +-----------+
    | label(v)  |
    +-----------+
    | Orders    |
    | Customers |
    | Stores    |
    | Products  |
    +-----------+
    
    
    opg-jshell-rdbms> query.accept("select id(v) from graph match (v:Customers)");
    +-------+
    | id(v) |
    +-------+
    | 40015 |
    | 40099 |
    | 40264 |
    | 40271 |
    | 40281 |
    | 40334 |
    | 40385 |
    | 40105 |
    | 40114 |
    | 40124 |
    | 40136 |
    | 40146 |
    | 40266 |
    | 40284 |
    | 40331 |  
    | 40030 |
    | 40067 |
    | 40082 |
    | 40120 |
    | 40154 |
    | 40174 |
    | 40323 |
    | 40091 |
    | 40348 |
    | 40374 |
    +-------+
    
    
    opg-jshell-rdbms> query.accept("select count(v) from graph match (v:Customers)");
    
    +----------+
    | count(v) |
    +----------+
    | 392      |
    +----------+
    
    opg-jshell-rdbms> query.accept("select s.store_name from graph match (c:Customers)->(o:Orders)->(s:Stores) where id(c)=40202");
    
    +--------------+
    | s.store_name |
    +--------------+
    | Online       |
    | Online       |
    | Online       |
    | Online       |
    | Online       |
    +--------------+
    
    opg-jshell-rdbms> query.accept("select s.store_name, o.order_id from graph match (c:Customers)->(o:Orders)->(s:Stores) where id(c)=40202");
    +---------------------------+
    | s.store_name | o.order_id |
    +---------------------------+
    | Online       | 294        |
    | Online       | 1473       |
    | Online       | 122        |
    | Online       | 996        |
    | Online       | 1519       |
    +---------------------------+
    
    
  
    opg-jshell-rdbms> query.accept("select s.store_name, o.order_id, p.product_name from graph match (c:Customers)->(o:Orders)->(s:Stores), (o:Orders)-[e:Order_Has_Product]->(p:Products) where id(c)=40202");
   
    
    +-----------------------------------------------------+
    | s.store_name | o.order_id | p.product_name          |
    +-----------------------------------------------------+
    | Online       | 1473       | Women's Skirt (Red)     |
    | Online       | 1473       | Women's Dress (Black)   |
    | Online       | 122        | Boy's Shorts (Blue)     |
    | Online       | 294        | Women's Trousers (Blue) |
    | Online       | 1473       | Boy's Sweater (Green)   |
    | Online       | 122        | Boy's Shirt (Black)     |
    | Online       | 122        | Boy's Jeans (Black)     |
    | Online       | 294        | Women's Jacket (Black)  |
    | Online       | 996        | Boy's Socks (Grey)      |
    | Online       | 1519       | Boy's Socks (Black)     |
    +-----------------------------------------------------+
    opg-jshell-rdbms>





   
   
   
   
   

