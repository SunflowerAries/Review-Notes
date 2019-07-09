# <center>Notes before midterm</center>

[TOC]

## 关系运算

### 关系代数

- **连接**

  $R \Join_{i \theta j} S =\sigma_{i \theta (r+j)}(R \times S)$

- **自然连接**

  $R \Join S =\pi_{i_1, ...,i_m}(\sigma_{R.A_1 = S.A_1 \and ... \and R.A_k = S.A_k}(R \times S))$

- **除法**

  $T = \pi_{1,2,...,r-s}(R) \\ W=(T \times S) - R \\ V = \pi_{1,2,...,r-s}(W) \\ R \div S = T - V$

- **改名**

  将R改名为S	$\rho_{S(A_1,..,A_n)}(R)$

- **半连接**

  $R \ltimes S \equiv \pi_R(R \Join S)$

### 元组关系演算

### 域关系演算

### 关系逻辑

**安全性**：出现在规则中任意地方的变量必须出现在某个非求反的关系子目标中。即在头部、求反关系字母表或任何算术子目标中出现的变量，必须出现在非求反的关系子目标中。

**关系逻辑与关系代数的差异**：规则中带有否定时，关系逻辑比关系代数更富于表现力。只有在规则被约束为安全的、非递归的、在带有某些否定的情况下，关系代数与关系逻辑等价。

## SQL操作

```
创建模式
Create schema <模式名> authorization <用户名>
撤销模式
Drop schema <模式名> [cascade | restrict]
创建表
Create table SC(S# char(4),
				C# char(4),
				Grade smallint,
				Primary key(S#, C#),
				Foreign key(S#) Referrences S(S#),
				Foreign key(C#) Referrences C(C#),
				Check(Grade between 0 and 100));
修改表
Alter table S Add/Modify S# Char(6);
Alter table S Drop S# [cascade | restrict];
Drop table S [cascade | restrict];
匹配
Select Sname
from S
where Sname like 'D%';	like 'ab \ % cd%' escape '\' 匹配所有以"ab%cd"开头的字符串
数据插入
Insert into SC(S#, C#, Grade)
	values('S4', 'C6', 90);
Insert into SC(S#, C#)
	table SC4;
Insert into S_grade(S#, AVG_grade)
	select S#, AVG(Grade)
		from ....
数据删除
Delete from SC
where C# = 'C4'
	and grade < ...
数据修改
Update C
set teacher = 'Wu'
where C# = 'C5';
SQL递归
With recursive W(C#, PC#) as
		(select C#, PC# from course)
	union
		(select W1.C#, W2.PC# 
		 from W as W1, W as W2
		 where W1.PC# = W2.C#)
	select PC# from W where C# = 'C4';
视图创建
Create View student_grade(S#, sname, gname, grade)
	as select ...
视图撤销
Drop view student_grade
视图更新
对视图的更新只能在由单个基本表只使用选择、投影操作导出且包含基本表的主键的视图上进行。
即对单个基本表使用了分组和聚集操作的也不行。
```

## 关系数据库的规范化设计

### 无损分解

Chase算法

### 范式

- 第一范式(1NF)：原子性
- 第二范式(2NF)：非主属性（不属于候选键的属性）局部依赖
- 第三范式(3NF)：非主属性传递依赖于主属性
- BCNF：主属性传递依赖于候选键

