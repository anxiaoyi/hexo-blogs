---
title: 'mysql command'
layout: post
date: 2015-08-20
tags:
- mysql
categories:
- software
---

# MySql Command

```sql

SHOW VARIABLES WHERE Variable_name LIKE 'character\_set\_%' OR Variable_name LIKE 'collation%';

ALTER TABLE mytable CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;

ALTER TABLE mytable CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;

SET NAMES utf8mb4;

ALTER DATABASE mydatabase CHARACTER SET = utf8mb4 COLLATE = utf8mb4_unicode_ci;

ALTER TABLE mytable CHANGE message_body message_body VARCHAR(512) CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;

show full columns from mytable;

```
