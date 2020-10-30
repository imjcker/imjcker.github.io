---
layout: post
title: Oracle All in One
category: DB
tags: oracle
---

# 2018-10-06-oracle

查询当前用户所有表 SELECT \* FROM user\_tab\_comments;

1. 函数 function

```sql
create or replace function f_demo001 return int as
var_count int;
begin
  SELECT COUNT(*) into var_count FROM emp;
  return var_count;
end f_demo001;
--test
SELECT f_demo001 cnt from dual;
```

1. 基础运用 Basics

```sql
Unlock SCOTT
          ALTER USER SCOTT ACCOUNT UNLOCK;

Show all tables 
     select * from user_tab_comments;
# 级联查询
SELECT dwbm  FROM xt_zzjg_dwbm@tyyw_db.net START WITH dwbm = '123123' CONNECT BY PRIOR dwbm = fdwbm
```

1. 存储过程 Procedure

```sql
create or replace procedure sp_demo001(arg_empNo in int，arg_result out varchar2)
as
var_cnt int;
Begin
   Select count(*) into var_cnt from emp;
   If (var_cnt > 0) then
      arg_result := '有匹配的值';
   Else
      arg_result := '无匹配的值';
   End if;
Exception
   When others then
      Rollback;
End;
```

1. 导库

```sql
--创建表空间
CREATE TABLESPACE jcker DATAFILE 'C:\app\Administrator\oradata\jcker\jcker.dbf' size 500M autoextend on next 10M maxsize 30G LOGGING EXTENT MANAGEMENT LOCAL SEGMENT SPACE MANAGEMENT AUTO;
--创建用户并分配权限
create user jcker identified by jcker default tablespace jcker temporary tablespace temp;
alter user jcker quota unlimited on jcker quota 0M on system;
--设置用户密码永不过期
ALTER PROFILE DEFAULT LIMIT PASSWORD_LIFE_TIME UNLIMITED;
GRANT connect, resource to jcker;
GRANT create public synonym to jcker;
GRANT create synonym to jcker;
GRANT create any table to jcker;
GRANT create any view to jcker;
GRANT debug connect session to jcker;
GRANT debug any PROCEDURE to jcker;
GRANT create tablespace to jcker;
GRANT alter tablespace to jcker;
GRANT create any directory to jcker;
GRANT create database link to jcker;
GRANT execute on dbms_lock to jcker;
GRANT datapump_exp_full_database to jcker;
GRANT datapump_imp_full_database to jcker;
-- 控制台执行导库
imp userid=jcker/jcker full=y file=C:\app\Administrator\oradata\jcker\KSS20180710.dmp ignore=Y log=C:\Users\Administrator\Desktop\log.log
```

1. 编码 Oracle字符编码查询

```sql
SELECT t.* FROM nls_database_parameters t;
SELECT t.* FROM nls_instance_parameters t;
SELECT t.* FROM v$version t;


SELECT userenv('language') from dual;--查询Oracle server字符集

SELECT nls_charset_name(to_number('0354','xxxx')) from dual;--通过查询dmp文件的二进制格式的第2，3位，判断dmp文件的编码。



alter system set "_system_trig_enabled"=false;
```

1. 哈啊哈

