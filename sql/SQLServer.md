# SQL Server 编码规范指引

## 1. 指引制定原则

* 1．方便代码的交流和维护。
* 2．不影响编码的效率，不与大众习惯冲突。
* 3．使代码更美观、阅读更方便。
* 4．使代码的逻辑更清晰、更易于理解。

## 2. 逻辑对象的命名规范

用于规范数据库对象的命名，以便于区分不同类型的数据库对象。命名可采用英文或使用汉字简拼。采用英文命名时首字母大写，采用汉字简拼时所有字母大写。
数据对象类别标识:

| 数据对象类别 | 前缀	  |  
| ---          | ---    |  
| 表           | tb_    |  
| 视图         | view_  |  
| 触发器       | trig_  |  
| 存储过程     | proc_  |  
| 函数         | func_  |  
| 索引         | IX_    |  
| 主键         | PK_	  |  
| 外键         | FK_	  |  
		 
### 2.1 数据库命名
数据库的命名要求使用与数据库意义相关联的英文或者拼音大写首字母。例如：

协同办公项目 `X3_OA`、`X3_Platform`
资产管理数据库的命名为`ZCGL`。

### 2.2 数据库文件及目录
数据库主数据文件命名：`{database_name}.mdf`，文件组名称：PRIMARY
数据库事务日志文件命名：`{database_name_log}.ldf`
数据库文件组命名：主文件组PRIMARY，次文件组`{database_name_filegroup}`
其中：`{database_name}`为数据库实际名称

### 2.3 数据表命名

| tb  | _   | ×	 | ×  | ×  | ×  | ×  | ×  | ×  | ×  | ×  | × |  
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |--- |

_ 命名规则：tb(数据对象类别标识) ＋数据描述 ＋ 表类型(非必要) _
示例：tb_Account              帐号信息表
         tb_Role	          	    角色信息表

### 2.4 存储过程命名
 
proc	_	×	×	×	×	×	×	_	×	×	×	×	×	×	 
命名规则：proc(数据对象类别标识) ＋数据描述(非必要) + 处理描述 
示例：proc_News_GetPages     获取新闻分页列表的存储过程

### 2.5 视图命名
 
view	_	×	×	×	×	×	×	×	×	×	×	×	×	 
命名规则：view(数据对象类别标识) ＋数据描述 
示例：view_Task  任务信息视图

### 2.6 函数命名
 
func	_	×	×	×	×	×	×	_	×	×	×	×	 
命名规则：F(数据对象类别标识) ＋数据描述(非必要)＋处理描述 
示例：func_GetOrganizationPathNameByOrganizationId  获取单位全称的函数
         func_GetTimestamp  获取时间戳的函数
### 2.7 索引命名
 
IX	_	×	×	×	×	×	×	×	×	×	×	 
命名规则：IX(数据对象类别标识) ＋ 表名 ＋ 字段名(非必要)
示例：IX_td_Account  用户信息表以Id为索引

### 2.8 触发器命名
 
trig	_	×	×	×	_	×	×	×	_	×	×	×	 
命名规则：TR(数据对象类别标识) ＋ 表名＋触发条件类别(I:Insert,U:Update,D:Delete) ＋ 触发顺序类别(A:After,B:Befor)  
示例：trig_tb_Account_Update_After 在账户信息表的Update触发器(在Update之前触发)

### 2.9 主键命名
单主键
 
PK	_	×	×	×	×	×	×	×	×	×	×	 
命名规则：PK(数据对象类别标识) ＋表名＋ 字段名(非必要)
示例：PK_tb_Account  账户信息表的主键

### 2.10 外键命名
 
FK	_	×	×	×	×	×	×	×	×	×	×	 

**命名规则: FK(数据对象类别标识) ＋ 外键表名 ＋ 字段名(非必要) ＋ 主键表名 ＋ 字段名(非必要)**

示例：
FK_tb_Account_Role_AccountId_tb_Account_Id  用户信息表的外键
     或者FK_tb_Account_Role_tb_Account

## 3.可编程性编码规范

### 3.1 可编程性统一规范

#### 3.1.1 代码编写格式规范
* 1）一般设置TAB = 4， 并将TAB自动转换为空格；
* 2）每行最大限度为80个字符；
* 3）单行注释采用--，多行注释采用/* 注释内容 */ 的形式；
* 4）多个Begin…End语句嵌套时采用如下方式。
``` SQL
BEGIN /*1*/
    …
    BEGIN /*1.1*/
        …
        BEGIN /*1.1.1*/
            …
        END /*1.1.1*/

        BEGIN /*1.1.2*/
            …
        END /*1.1.2*/
    END /*1.1*/
END /*1*/
```
其中1表示第一级嵌套，1.1表示第二级嵌套，1.1.1表示第三级嵌套，1.1.2表示第三级的第二个嵌套 …，一般不要超过三级嵌套。

### 3.2 存储过程
#### 3.2.1 存储过程格式
``` SQL
USE <database_name>
GO
IF exist ()
  DROP PROCEDURE <usename>.<procedure_name>
GO
SET
SET
CREATE PROCEDURE <usename>.<procedure_name>
  [[(]@parameter_name datatype [OUTPUT]
  [,@parameter_name datatype [OUTPUT]]..[]]
  AS
BEGIN
    SQL_statements
END
GO
SET
SET
```

#### 3.2.2 存储过程标头备注
过程前应有文字说明，说明本过程是做什么的、调用者是谁、返回值的含义、参数的含义、输入数据库、输出数据库，每一步操作前有文字说明，说明该操作达到的目的。
格式为：
```SQL
/******************************************************************
概要说明：
  中文名称：
  用    途：
  数 据 库：
语法信息：
  输入参数：
  输出参数：
  调用举例：
外部联系：
  上级调用：
      下级调用：
      输         入：
      输         出：
功能修订：
      简要说明：
      修订记录：
            <修订日期> <修订人>：修改内容简要说明
                  <续简要说明>
            <修订日期> <修订人>：修改内容简要说明
                  <续简要说明>
******************************************************************/
```

#### 3.2.3 返回值
* 1）使用输出参数返回过程执行的相关信息。
* 2）过程结束前应调用return返回代码，作为存储过程的执行状态。

### 3.3 函数
#### 3.3.1 函数格式
```SQL
use <database_name>
go
if exist …
drop FUNCTION <usename>.<function_name>
go
create function dbo.Function_Name()
	returns <function_data_type, ,int>
as
begin
    <function_body, , RETURN >
END
```

#### 3.3.2 函数标头备注
函数前应有文字说明，说明本函数是做什么的、调用者是谁、返回值的含义、参数的含义、输入数据库、输出数据库，每一步操作前有文字说明，说明该操作达到的目的。
格式为:
```SQL
/******************************************************************
概要说明：
  中文名称：
  用    途：
  数 据 库：
语法信息：
  输入参数：
  输出参数：
  调用举例：
外部联系：
  上级调用：
  下级调用：
  输    入：
  输    出：
功能修订：
  简要说明：
  修订记录：
    <修订日期> <修订人>：修改内容简要说明
      <续简要说明>
    <修订日期> <修订人>：修改内容简要说明
      <续简要说明>
******************************************************************/
```

### 3.4 触发器
#### 3.4.1 触发器格式
```SQL
IF EXISTS (SELECT name FROM sysobjects
WHERE name = N'<trigger_name, sysname, trig_test>'
		AND type = 'TR')
	DROP TRIGGER <trigger_name, sysname, trig_test>
GO

CREATE TRIGGER <trigger_name, sysname, trig_test>
  ON <table_or_view_name, sysname, pubs.dbo.sales>
INSTEAD OF INSERT
AS
BEGIN
     RAISERROR (50009, 16, 10)
END
GO
```

#### 3.4.2 触发器标头备注

触发器前应有文字说明，说明本触发器是做什么的、作用表是哪些、触发器类型等，每一步操作前应有文字说明，说明该操作达到的目的。
格式为：
```SQL
/******************************************************************
概要说明：
  中文名称：
  用    途：
  数 据 库：
  触发器类型：
语法信息：
  输入参数：
  输出参数：
  调用举例：
外部联系：
  上级调用：
  下级调用：
  输    入：
  输    出：
功能修订：
  简要说明：
  修订记录：
    <修订日期> <修订人>：修改内容简要说明
      <续简要说明>
    <修订日期> <修订人>：修改内容简要说明
      <续简要说明>
******************************************************************/
```

## 4.数据库编程技巧

### 4.1子查询与联结查询之比较
子查询与联结查询一般能互相替换，只不过子查询的效率较低，但是可读性比较好；而联接查询效率较高，可读性差，但有一种情况不能使用联接查询，即比较之间涉及到聚合函数，如使用avg，sum等函数时，只能使用子查询。例：
```SQL
SELECT 
    titles.title, titles.price
FROM titles JOIN sales ON sales.title_id = titles.title_id
WHERE sales.qty > (SELECT AVG(qty) FROM sales)
```

### 4.2大数据表联结查询技巧
大数据表间的联结查询通过一个SQL查询完成的效率较低，且联结条件和查询条件多间可读性比较差；可由一个或两个大数据表按最大过滤条件生成一个临时表（#.....），然后由此临时表联结生成，示具体情况甚至可一层层往下过滤--联结--过滤—联结，最终生成所须的结果集。

### 4.3聚类索引和非聚类索引之比较
#### A) 返回范围内的数据，返回如姓名范围或日期范围的订货量，一般使用聚类索引。因为聚类索引已经包含了经过分类排序的数据，聚类索引只需找到要检索的所有数据中的开头和结尾数据。
#### B) 列中有一个或极少的不同值，一般不使用任何索引。例：
```SQL
SELECT * FROM FewUniques WITH(INDEX(0)) WHERE Status = 'Inactive'
-- Status I/O from table scan access (comment was Added), Scan count 1,logical reads 45, physical reads 0
SELECT * FROM FewUniques WITH(Index(inFewUniquesStatus)) WHERE Status = 'Inactive'
-- Status I/O from table scan access (comment was Added), Scan count 1,logical reads 5018, physical reads 0
```
#### C) 小数目的不同值，一般采用聚类索引。
```SQL
select * 
  from FewUniques WITH(Index(0)) 
 where Status = ‘Inactive’
-- Status I/O from table scan access (comment was Added), Scan count 1,logical reads 16, physical reads 0
create  clustered index icFewUniquesStatus on 
  FewUniques(Status) GO

select * 
  from FewUniques WITH(Index(inFewUniquesStatus)) 
 where Status = ‘Inactive’
-- Status I/O from table scan access (comment was Added), Scan count 1,logical reads 3, physical reads 0
```
#### D) 大数目的不同值和更新索引列数据，当不同值的数目增加并达到表中行的数目时，一般选择使用非聚类索引。
#### E) 返回行数量较少以及频繁更新的列，一般使用非聚类索引。
#### F) 复合索引的优缺点
#### G) 在大多数情况下都使用相同的条件列组合应考虑建一复合索引
#### H) 在大多数情况下都使用不同的条件列组合则建多个单一索引或多个小的复合索引
#### I) 不单独进行搜索的列决不首先排列在索引中

### 4.4事务封装使用
事务封装的程序段和执行时间尽量短。

### 4.5动态光标使用
尽量少用光标循环，在大多数条件下可转换成联结查询。

### 4.6存储过程的设置选项使用
存储过程开头尽量加上SET NOCOUNT ON，减少不必要的“受作用行”等信息返回到客户端，不然ADO查询会出错，同时减少网络流量；

在返回结果集的SQL语句前使用SET ROWCOUNT 20000 ，防止出现无条件查询时太大的数据集返回到客户端。

