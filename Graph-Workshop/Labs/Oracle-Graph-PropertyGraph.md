<br>

**What Are Property Graphs?** 

A property graph consists of a set of objects or vertices, and a set of arrows or edges connecting the objects. Vertices and edges can have multiple properties, which are represented as key-value pairs.

Each vertex has a unique identifier and can have:


•	A set of outgoing edges

•	A set of incoming edges

•	A collection of properties

Each edge has a unique identifier and can have:

•	An outgoing vertex

•	An incoming vertex

•	A text label that describes the relationship between the two vertices

•	A collection of properties


The following figure illustrates a very simple property graph with two vertices and one edge. The two vertices have identifiers 1 and 2. Both vertices have properties name and age. The edge is from the outgoing vertex 1 to the incoming vertex 2. The edge has a text label knows and a property type identifying the type of relationship between vertices 1 and 2.

![](./images/IMGG1.PNG) 

**Property Graph Prerequisites:**

The requirements for using the property graph features are the following.

•	The Oracle Graph Server and Graph Client must be installed.

•	max string size must be enabled.

•	AL16UTF16 (instead of UTF8) must be specified as the NLS NCHAR CHARACTERSET.

•	AL32UTF8 (UTF8) should be the default character set,  but  AL16UTF16 must be the NLS NCHAR CHARACTERSET.


**Oracle Graph Server and Client:**

It is a software package for use with the Property Graph feature of Oracle Database. Oracle Graph Server and Client includes the high speed in-memory analytics server (PGX) and client libraries required for graph applications.

Oracle Graph Client: A zip file containing Oracle Graph Client.

Oracle Graph Server: An rpm file containing an easy to deploy Oracle Graph Server.

**Note:** Graph client and Sever installation is completed and the setup is ready for use.
 
 