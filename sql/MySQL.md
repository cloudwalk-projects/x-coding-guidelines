# MySQL 编码规范指引

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
| 索引         | ix_    |  
| 主键         | pk_	  |  
| 外键         | fk_	  |  
		 
### 2.1 数据库命名
数据库的命名要求使用与数据库意义相关联的英文或者拼音大写首字母。例如：

协同办公项目 `x3_oa` 或者 `x3_platform`
资产管理数据库的命名为`ZCGL`。

### 2.2 数据表命名

| tb   | _   | ×	| ×  | ×  | ×  | ×  | ×  | ×  | ×  | ×  | × |  
| ---  | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |--- |

命名规则：tb(数据对象类别标识) ＋数据描述 ＋ 表类型(非必要) _
示例：tb_account              帐号信息表
      tb_role	          	    角色信息表
      tb_account_role	        帐号和角色信息关系表

### 2.3 视图命名
 
| view | _   | ×	| ×  | ×  | ×  | ×  | ×  | ×  | ×  | ×  | × |  
| ---  | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |--- |

命名规则：view(数据对象类别标识) ＋数据描述 
示例：view_task  任务信息视图

### 2.4 函数命名
 
| func | _   | ×	| ×  | ×  | ×  | ×  | ×  | ×  | ×  | ×  | × |  
| ---  | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |--- |
	 
命名规则：F(数据对象类别标识) ＋数据描述(非必要)＋处理描述 
示例：func_GetOrganizationPathNameByOrganizationId  获取单位全称的函数
         func_GetTimestamp  获取时间戳的函数
### 2.5 索引命名
 
| ix   | _   | ×	| ×  | ×  | ×  | ×  | ×  | ×  | ×  | ×  | × |  
| ---  | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |--- |
	 
命名规则：ix(数据对象类别标识) ＋ 表名 ＋ 字段名(非必要)
示例：ix_td_account  用户信息表以Id为索引

### 2.6 主键命名
单主键
 
| pk   | _   | ×	| ×  | ×  | ×  | ×  | ×  | ×  | ×  | ×  | × |  
| ---  | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |--- |
	 
命名规则：PK(数据对象类别标识) ＋表名＋ 字段名(非必要)
示例：PK_tb_Account  账户信息表的主键

### 2.10 外键命名
 
| fk   | _   | ×	| ×  | ×  | ×  | ×  | ×  | ×  | ×  | ×  | × |  
| ---  | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |--- |

**命名规则: FK(数据对象类别标识) ＋ 外键表名 ＋ 字段名(非必要) ＋ 主键表名 ＋ 字段名(非必要)**

示例：
fk_tb_account_role_accountId_tb_Account_Id  用户信息表的外键
     或者 fk_tb_Account_Role_tb_Account

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

### 3.3 函数
#### 3.3.1 函数格式
```SQL
use <database_name>
go
if exist …
drop FUNCTION <function_name>
go
create function function_name()
	returns <function_data_type, int>
as
begin
    <function_body, RETURN >
end
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
