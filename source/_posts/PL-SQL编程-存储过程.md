title: PL/SQL编程-存储过程
author: 学亮
tags:
  - plsql编程
categories: []
date: 2019-04-28 16:23:00
---
> 存储过程实际上就是一种命名的PL/SQL程序块。

**创建存储过程**

> 创建存储过程需要使用procedure关键字。
> 创建存储过程不需要使用DECLARE关键字，转而使用CREATE/REPLACE关键字。

```
create or replace procedure pro_insertDept 
is
begin
	insert into dept values(11,'软件研发中心','xijing');
	commit;
	dbms_output.put_line('插入新纪录成功！');
end pro_insertDept;
```

> 在sql*plus环境，使用EXECUTE命令执行pro_insertDept存储过程：

```
execute pro_insertDept;
```

> 在PL/SQL代码块中调用存储过程pro_insertDept:

```
begin
	pro_insertDept;
end;
```
**存储过程的模式参数**

> 存储过程的参数模式包括in   out和in out等3种。

**in模式参数**

```
create or replace prodecure pro_insertDept(
	num_deptno in number,
	var_ename in vatchar2,
	var_loc in varchar2
)
is
begin
	insert into dept values(num_deptno,var_ename,var_loc);
	commit;
end pro_insertDept;
```

> 需要注意的是：参数的类型不能指定长度。

**①按指定名称传递**

```
begin
	pro_insertDept(var_ename=>'采购部',var_loc=>'西京',num_deptno=>18);
end;
```

**②按位置传递**

```
begin
	pro_insertDept(28,'工程部','武汉');
end;
```

> 用户提供的参数值顺序必须与存储过程定义的参数顺序相同。
> 
> ps:可以使用DESC命令来查看存储过程中的参数信息。

**③混合方式传递**

```
begin
	pro_insertDept(38,var_loc=>'光谷',var_ename=>'光电信息产业部');
end;
```

> 注意：在某个位置使用了“按指定名称传递”的方式传入参数值后，其后面的参数值也要使用“按指定名称传递”，因为“按指定名称传递”或许已经破坏了参数原始的定义顺序。


**in参数的默认值**

```
create or replace procedure pro_insertDept(
	num_deptno in number,
	var_dname in varchar2 default '综合部',
	var_loc in varchar2 default '武汉'
) is
begin
	insert into dept values(num_deptno,var_dname,var_loc);
end;
```

> 调用存储过程pro_insertDept，只向该存储过程传入两个参数值。

```
declare
	row_dept dept%rowtype;
begin
	pro_insertDept(57,var_loc=>'江夏');
	commit;
	select * into row_dept from dept where deptno=57;
	dbms_output.put_line('部门名称是：'||row_dept.dname||',位置是：'||row_dept.loc);
end;
```


