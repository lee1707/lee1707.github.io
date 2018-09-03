---
layout: post
title: Error06 com.mysql.jdbc.exceptions.jdbc4.MySQLSyntaxErrorException
category: Errors 
tags: [syntaxerror, badsqlgrammerexception, error]
---

com.mysql.jdbc.exceptions.jdbc4.MySQLSyntaxErrorException

# 에러 

```
org.springframework.jdbc.BadSqlGrammarException: 
### Error updating database.  Cause: com.mysql.jdbc.exceptions.jdbc4.MySQLSyntaxErrorException: You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near 'order (productId, address, productName, price, imageUrl) VALUES ('000', 'address' at line 1
### The error may involve Order.insert-Inline
### The error occurred while setting parameters
### SQL: INSERT INTO order (productId, address, productName, price, imageUrl) VALUES (?, ?, ?, ?, ?)
### Cause: com.mysql.jdbc.exceptions.jdbc4.MySQLSyntaxErrorException: You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near 'order (productId, address, productName, price, imageUrl) VALUES ('000', 'address' at line 1
; bad SQL grammar []; nested exception is com.mysql.jdbc.exceptions.jdbc4.MySQLSyntaxErrorException: You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near 'order (productId, address, productName, price, imageUrl) VALUES ('000', 'address' at line 1
    at
    ```


Order.insert-Inline
```
<insert id="insert" parameterType="com.whiuni.fastshop.vo.OrderVO">
        INSERT INTO order (productId, address, productName, price, imageUrl) VALUES (#{productId}, #{address}, #{productName}, #{price}, #{imageUrl})
</insert>
```

# 원인

order는 MySQL의 예약어였다

> order is a reserved word in mysql, change the table name or surround it with single-quotes

`INSERT INTO 'order' (productId, address, productName, price, imageUrl) ...`

# 참고할 것
[MySQL reserved Keyword](https://dev.mysql.com/doc/refman/8.0/en/keywords.html)