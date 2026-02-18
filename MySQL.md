# MySQL

```
mysql -uroot -p  //with password
```

**注** MySQL默认自带database mysql

#### SQL

**注** 设置名称时，如果是普通名称可不加反引，加上反引号``后，可避免关键字冲突、接受特殊字符、接受数字开头、严格保留大小写

##### database

```
show databases;
drop database ...;
create database ... charset utf8;
use ...;
```

##### table

```
show tables;
drop table if exists ...;
create table ...(
	`id` int primary key not null auto_increment,   //auto_increment 自动增长
	`username` varchar(32), 
	`password` varchar(32)
)default charset=utf8mb4;  //utf8mb4支持emoji
desc ...;  //查看table结构
alter table ... rename ...;
alter table ... add `id` int;
alter table ... change `old_id` `new_id` int;
alter table ... modify `id` varchar(10);
alter table ... drop `id`;
```

###### 约束条件

```
create table `students`(`name` varchar(16),`sex` not null default '男'); //完整性约束 如果输入性别为空 默认为男
create table `students`(`id` int unique,`name` varchar(16) unique); //唯一性约束 unqie为唯一 可以有多个
create table `students`(`id` int ,`name` varchar(16),primary key(`id`,`name`)); //联合主键
create table `students`(`id` int primary key auto_increment); //自增
create table `students`(`id` int, `name` varchar(16), foreign key(class_id) references class(`id`));//外键
```

##### data

**注** [] 意为可选 是文档标记符号 非sql规范

```
select ... from ...;
insert into ...(`id`,`username`,`password`) values(1,"...","..."),(2,"...","...");
update ... 
set `id`="..."
[where];  //不加where条件则更新整表
delete from ...
[where];  //不加则删除整表
```

###### 查询约束条件

```
select concat('姓名',`name`,'年龄',`age`)as `stu_desc` from `students`; //使用别名显示 stu_desc可替换
select distinct `name`,`age` from `students`;  //去重查询
select * from students where age>=18;  // 单条件查询
select * from students where age>=18 and sex="男"; // 多条件查询
select * from students where age between 18 and 25; // 范围查询
select * from students where name in ('hhq','zxy'); // 包含查询
select * from students where name like "黄%";
//模糊查询 |"小%":小开头的|"%小":小结尾的|%小%":包含小的
// "小_":两个字符且小开头 (特定模糊查询)
```

###### 分组查询

```| max   | 最大值 |
| min max | 最小值 最大值|
| avg     | 平均值      |
| sum     | 总值       |
| count   | 计数       |`
```

```
select age,count(age) as "年龄人数" from students group by age;  //计数查询 as"年龄人数"是在给字段起别名
select age,count(age) as "年龄人数" from students group by age having count(age)>25;  //计数查询过滤
select age from students order by age asc;  //排序查询 升序 默认升序
select age from students order by age desc;  //排序查询 降序
select * from students limit 10;  //限数查询
```

###### 多表查询

```
select * from students,class where students.cls_id=class.id;
```
