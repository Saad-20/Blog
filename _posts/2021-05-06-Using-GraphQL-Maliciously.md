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

From the screenshot above, the only query that was interesting was Ping Query
The reason Ping query is interesting is because it can be used to create remote code execution and takeover the server :D   
Let's check what fields are available for the query   
<p align="center">
 <img src="https://raw.githubusercontent.com/Saad-20/Blog/master/assets/GRAPHQL/5_examining_ping_fields.png" />
</p>   
There are two availble fields that are being used those are ```ip``` & ```output```   
Let's give it a test of how it works. The query that i injected is: -   
{   
   Ping(ip:"127.0.0.1"){   
     ip   
     output   
  }   
}     
We get the output that it makes the call to the localhost.   
<p align="center">
 <img src="https://raw.githubusercontent.com/Saad-20/Blog/master/assets/GRAPHQL/6_Testing_ping_field.png" />
</p>   
let's try to break the query by inserting a semicolon and check if we can read the passwd file since the backend is a linux server running.The payload being used in the query is : -    
{   
   Ping(ip:"; cat /etc/passwd"){   
     ip   
     output   
  }   
}    
<p>
 <img src="https://raw.githubusercontent.com/Saad-20/Blog/master/assets/GRAPHQL/7_etc_passwd.png">
</p>   
We are able to read the passwd file. That is a good sign, let's start making our reverse shell and get an rce :p   
Let's open our ncat connection and start working on our payload.   
<p>
 <img src="https://raw.githubusercontent.com/Saad-20/Blog/master/assets/GRAPHQL/8_ncat_listener.png">
</p>   
<p>
 <img src="https://raw.githubusercontent.com/Saad-20/Blog/master/assets/GRAPHQL/9_reverse_shell.png">
</p>      

Reverse shell got executed and we got a remote code execution. Thus, compromising the server    
<p>
 <img src="https://raw.githubusercontent.com/Saad-20/Blog/master/assets/GRAPHQL/10_server_hacked.png">
</p>   
Now we got a reverse shell, we need to now escalate our privileges to become root. Lets send our linpeas script to the compromised server to check what abnomalies can be exploit to become root. We run ourserver where our linpeas is located.
