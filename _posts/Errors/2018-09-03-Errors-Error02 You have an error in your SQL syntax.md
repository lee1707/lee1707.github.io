---
layout: post
title: Error02 You have an error in your SQL syntax
category: Errors 
tags: [sqlsyntaxerror, error]
---

# Today I learned
Resolution for Errors - SQL Syntax Error


# #SQL Syntax Error
Symptoms
-----------------
```
You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near 'where id ='null'' at line 1
```

Tried
-----------------
Add this code to print log.
```
 String query;
 if(Objects.isNull(id)) {
    query = "SELECT `password` FROM `userinfo` WHERE id is null"; 
 } else {
    query = "SELECT `password` FROM `userinfo` WHERE id = '"+id+"'"; 
 }
 System.out.println(query);
```

Cause
-----------------
```
ResultSet rs = stmt.executeQuery("select password" +
                                 "from userinfo where id = '" + id + "';"); 
```
1. There were no space befor `from`
2. I forgot to recompile this java file and restart tomcat.

Resolution
-----------------
I changed code to this.
```
"SELECT `password` FROM `userinfo` WHERE id = '"+id+"'"; 
```
Also, recompile this file.