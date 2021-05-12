---
title: Using GraphQL Maliciously
published: true
---
Hey Hackers, Today's article will be about GraphQL Lab by tryhackme. Before we get started with the writeup, let's just get to know what is GraphQL and how we can exploit it :D
<p align="center">
  <img src="https://raw.githubusercontent.com/Saad-20/Blog/master/assets/GRAPHQL/Graphql_logo.png"/>
</p>

**What is GraphQL?**   
GraphQL is a query language that allows to access data from the database via single API endpoint. As compared to REST API, where there are seperate endpoint for each calling the user makes. With GraphQL, we only need one endpoint to access all data.   
There are two operations in GraphQL API, they are: -   
1. Query   
2. Mutation   

Query is used to read data while mutation is used to modify data. You can learn more about GraphQL at this link ```https://graphql.org```   

Well now you know what GraphQL, lets' get started.   

Start up the machine      
<p align="center">
  <img src="https://raw.githubusercontent.com/Saad-20/Blog/master/assets/GRAPHQL/1_start_machine.png"/>   
</p>  

After starting the machine, we need to perform some enumeratation   
<p align="center">
  <img src="https://raw.githubusercontent.com/Saad-20/Blog/master/assets/GRAPHQL/2_nmap.png"/>
</p>  
   
By using, nmap we got to know port 80 is open. Let's check what is on port 80.   
<p align="center">
  <img src="https://raw.githubusercontent.com/Saad-20/Blog/master/assets/GRAPHQL/3_open_port_80.png"/>
</p>  

On port 80, we found that Graphiql interface is enabled, thus allowing us to run graphql query. We wrote the graphql query in the interface in order to know what queries are available   
<p align="center">
  <img src="https://raw.githubusercontent.com/Saad-20/Blog/master/assets/GRAPHQL/4_schema_fields.png"/>
</p>
