# MySQL笔记

- [MySQL笔记](#mysql笔记)
  - [查询去除重复的结果集](#查询去除重复的结果集)
    - [限制结果集](#限制结果集)
      - [使用完全限定的表名查询](#使用完全限定的表名查询)
      - [排序查询结果集](#排序查询结果集)
        - [指定排序方向关键字 desc acs](#指定排序方向关键字-desc-acs)
        - [order by 与 limit](#order-by-与-limit)
      - [过滤数据](#过滤数据)
        - [where 子句操作符](#where-子句操作符)
      - [空值检查](#空值检查)
      - [组合where 子句](#组合where-子句)
        - [逻辑操作符 and or`](#逻辑操作符-and-or)
        - [操作符 in](#操作符-in)
        - [操作符 not](#操作符-not)
        - [like操作符](#like操作符)
        - [_操作符](#_操作符)
      - [用正则表达式进行查询](#用正则表达式进行查询)
        - [基本字符匹配](#基本字符匹配)
        - [进行 or 匹配](#进行-or-匹配)
        - [匹配范围](#匹配范围)
        - [匹配特殊字符](#匹配特殊字符)
        - [匹配多个实例](#匹配多个实例)
        - [定位符](#定位符)
      - [创建计算字段](#创建计算字段)
        - [计算字段](#计算字段)
        - [去除空格](#去除空格)
        - [使用别名](#使用别名)
        - [执行算数计算](#执行算数计算)
      - [使用数据处理函数](#使用数据处理函数)
        - [文本处理函数](#文本处理函数)
        - [日期时间处理函数](#日期时间处理函数)
        - [数值处理函数](#数值处理函数)
      - [汇总数据](#汇总数据)
        - [聚集函数](#聚集函数)
        - [组合聚集函数](#组合聚集函数)
      - [分组数据](#分组数据)
        - [创建分组](#创建分组)
        - [过滤分组](#过滤分组)
        - [子句顺序](#子句顺序)
      - [使用子查询](#使用子查询)
        - [子查询](#子查询)
        - [作为计算字段使用子查询](#作为计算字段使用子查询)
      - [联结表](#联结表)
        - [内部联结](#内部联结)
        - [左联结](#左联结)
        - [右联结](#右联结)
      - [创建高级联结](#创建高级联结)
        - [使用表别名](#使用表别名)
        - [自联结](#自联结)
        - [使用带聚集函数的联结](#使用带聚集函数的联结)
      - [组合查询/复合查询](#组合查询复合查询)
        - [使用union](#使用union)
      - [全文本搜索](#全文本搜索)
        - [要点](#要点)
        - [进行全文搜索](#进行全文搜索)
        - [使用查询扩展](#使用查询扩展)
        - [布尔文本搜索](#布尔文本搜索)
        - [布尔全文搜索操作符](#布尔全文搜索操作符)
        - [注意事项](#注意事项)
      - [插入数据](#插入数据)
        - [数据插入](#数据插入)
        - [插入多个行](#插入多个行)
        - [将查询出的数据插入到另一个表中](#将查询出的数据插入到另一个表中)
      - [更新和删除数据](#更新和删除数据)
        - [更新数据](#更新数据)
        - [update 使用子查询](#update-使用子查询)
        - [删除数据](#删除数据)
      - [创建和操控表](#创建和操控表)
        - [创建表](#创建表)
        - [not null](#not-null)
        - [主键](#主键)
        - [auto_increment](#auto_increment)
        - [last_insert_id](#last_insert_id)
        - [指定默认值](#指定默认值)
        - [引擎类型](#引擎类型)
        - [更新表](#更新表)
        - [删除表](#删除表)
        - [重命表](#重命表)

#### 查询去除重复的结果集

`使用DISTINCT关键字`

```mysql
SELECT DISTINCT vend_id FROM products;
```

#### 限制结果集

`使用LIMIT关键字`

```mysql
SELECT prod_name FROM products limit 5;
#只查询5条
select prod_name from products limit 3,5;
# 从查询结果集的第3条开始取出 5条；
select prod_name from products offset 3 limit 5;
#等同于上面的查询语句
```

#### 使用完全限定的表名查询

```mysql
select products.prod_name from chasing.products;
# 查询 从chasing数据库中的产品表中查询产品表中的产品名称字段
```

#### 排序查询结果集

`使用子句 order by`

```mysql
  select prod_name from products order by prod_name;
  #从产品表中查询所有产品名称并对产品名称字段排序 默认排序规则为升序 （小的在前面）
  select prod_name,prod_price,prod_id from products order by prod_price,prod_name;
  #同时对产品价格和产品名称进行排序
  #会对产品价格进行升序排序，如果出现相同的价格时，才会对产品名称进行排序，如果产品价格都是唯一的，那么不会对产品名称进行排序
```

##### `指定排序方向关键字 desc acs` 

```mysql
  #升序排序 ACS : A-Z 默认排序规则
  #降序排序 DESC : Z-A
  select prod_name from products order by prod_name desc;
  #对产品名称进行降序排序
  select prod_name,prod_price from products order by prod_price desc ,prod_name;
  #对产品价格使用降序排序，产品名称使用升序排序
  #desc 关键字 只作用于 它前面的 字段生效 
```

##### `order by 与 limit`

```mysql
  select prod_price from products order by prod_price desc limit 1;
  # 查询产品表中最贵的一个商品
  # 子句的顺序
  # from --- order by desc --- limit --- offset 
  
```

#### 过滤数据

`使用子句 where`

```mysql
select prod_name,prod_price from products where prod_price = 2.50;
#查询产品价格等于2.50 的全部商品
# 子句的顺序
# select --- from --- where --- order by desc --- limit --- offset 
```

##### `where 子句操作符`

| 操 作 符 | 说 明              |
| -------- | ------------------ |
| =        | 等于               |
| < >      | 不等于             |
| !=       | 不等于             |
| <        | 小于               |
| >        | 大于               |
| <=       | 小于等于           |
| =>       | 大于等于           |
| between  | 在指定的俩个值之间 |
|          |                    |

```mysql
select prod_name from products where product_name = 'dory';
# 查询结果 不区分大小写 ，结果集可能出现 Dory DORY dory
select prod_price from products where product_price <> 10;
#查询产品价格不等于10 的所有产品
select prod_price form products where between product_price 4 and 10;
#查询产品价格在4 - 10 之间的所有产品
```

#### 空值检查

`使用子句 is null`

```mysql
select prod_name from products where prod_price is null;
#查询所有价格为空值得产品名称
# 空值是只该字段为空 未赋值过 无默认值 ，它与 0 空字符串 空格 不同
```

#### 组合where 子句

##### 逻辑操作符 and or`

```mysql
select prod_id,prod_price,prod_name from products where vend_id = 1003 and prod_price <= 10;
# 查询产品表 vend_id 等于 1003 并且 prod_price 小于等于10的全部商品；

#如果要查询 多个并且条件 和一个或者条件的语句如何查询？
select prid_name,prod_price from products where (vend_id = 1002 or vend_id = 1003) and prod_price >= 10;
# 查询商品制造号 为 1002 或者 1003 的 并且 价格要小于等于10 的全部产品
#（）计算次序高于 and 和 or
```

##### 操作符 in

```mysql
select prod_name,prod_price from products where vend_id in (1002,1003) order by prod_name;
# in 最大的优点是可以包含其他select 语句 动态建立where子句
```

##### 操作符 not 

```mysql
# where 子句中的not 操作符只有一个功能 否定它之后所跟的任何条件
select prod_name,prod_price from products where vend_id not in (1002,1003) order by prod_name;
# 查询 产品制造号不为 1002 和 1003 的所有产品 并使用降序排序产品名称

```

##### like操作符

```mysql
select prod_id,prod_name from products where prod_name like 'jet%';
# 必须指定like 关键字 'jet%' 表示查询匹配jet开头的全部产品
select prod_id,prod_name from products where prod_name like '%jet%';
# 表示查询包含jet的字符串的全部产品
select prod_id,prod_name from products where prod_name like 's%e';
# 表示查询开头为s 结尾为e的结果 包括se
# % 不能匹配 NULL 值

```

##### _操作符

```mysql
select prod_id,prod_name from products where prod_name like 's_e';
#_只能匹配单个字符 如 sse sxe
#% 能匹配0-n 个字符 
```

#### 用正则表达式进行查询

##### 基本字符匹配

```mysql
select prod_name from products where prod_name regexp '.000' order by prod_name;
# 使用关键字 regexp 
#regexp 后面跟的为正则表达式规则
# .表示匹配任意字符 查询的结果可能为2000 x000
# MySQL 中的正则表达式匹配不区分大小写
select prod_name from products where prod_name regexp binary 'Jetpack .000';
#如果要精确匹配大小写 使用binary 关键字
```

##### 进行 or 匹配

```mysql
select prod_name from products where prod_name regexp '100|200|300' order by prod_name;
# 查询产品名称包含 100 或 包含200 或 包含300 的所有产品名称
```

##### 匹配范围

```mysql
select prod_name from products where prod_name regexp '[a-z] ton';
# 匹配结果如 a ton b ton
```

##### 匹配特殊字符

```mysql
select prod_name from products where prod_name regexp '\\.' order by prod_name;
#匹配产品名称包含.的结果 
#\\ 为转义字符
# \\\ 为匹配 字符 \
```

##### 匹配多个实例

```mysql
select prod_name from products where prod_name regexp '\\([0-9 sticks?\\])' order by prod_name;
# [0-9] 匹配任意数字
# sticks?  ?的意思是前一个字符出现1次或0次 所以就是 stick 也可以 sticks 也可以
# 可能匹配到的结果为 1 stick 、 0 sticks

select prod_name from products where prod_name regexp '[[:digit:]]{4}' order by prod_name;

#[:digit:] 匹配任意数字
#{4}要求前面的字符出现4次
#可能匹配到的结果为 1000 2313 2314
```

##### 定位符

```mysql
select prod_name from products where prod_name regexp '^[0-9\\.]' order by prod_name;
# 匹配以任意数字加.开头的所有产品名称
# 可能匹配的结果为 1.23 0.xx
```

#### 创建计算字段

##### 计算字段

```mysql
select Concat(vend_name,'(',vend_country,')') from vendors order by vend_name;
# concat 拼接函数
# 把查询结果中的 vend_name 字段和 vend_country 拼接起来
# concat函数 可以拼接多个字符串 使用, 分割要拼接的字符串

```

##### 去除空格

```mysql
select Concat(RTrim(vend_name),'(',RTrim(vend_country),')') from vendors order by vend_name;
#RTrim 函数为去除字符串右边的多余空格
#LTrim 函数为去除字符串左边的多余空格
#Trim 函数为去除字符串俩边的多余空格
```

##### 使用别名

```mysql
select Concat(RTrim(vend_name),'(',RTrim(vend_country),')') as vend_title from vendors order by vend_name;
# 上面语句将处理后的字段 定义别名为 vend_title 
```

##### 执行算数计算

```mysql
select prod_id,quantity,item_price,quantity*item_price as expanded_price from orderitems where order_num = 20005;
#上面语句将数据库中数量字段和单价字段乘法运算后 别名为 expanded_price 字段
```

#### 使用数据处理函数

##### 文本处理函数

```mysql
select vend_name,Upper(vend_name) as vend_name_upcase from vendors order by vend_name;
# Upper() 转换为大写
# Left() 返回串左边的字符
# Length() 返回串的长度
# Locate() 找出串的一个子串
# Lower() 转换为小写
# Right() 返回串右边的字符
# Soundex() 返回串的 SOUNDEX值 匹配所有发音类似于指定字符串的结果
# SubString() 返回子串的字符

```

##### 日期时间处理函数

```mysql
select cust_id,order_num from orders where Date(order_date) = '2005-09-01';
# 查询oder_date 日期等于 2005-09-01 的结果
# order_data 的数据类型应该是Datetime 类型
# 这样可以查询出 这一天的订单 就算是储存结果为 2005-09-01 12:30:22 也可以查询出来

select cust_id,order_num from orders where Date(order_date) between '2005-09-01' and '2005-09-30';
# 匹配日期范围内的数据

select cust_id,order_num from orders where Year(order_date) = 2005 and Month(order_date) = 9;
# 匹配 2005年 9月分的数据
```

##### 数值处理函数

```mysql
# Abs() 绝对值
# Mod() 取余
# Rand() 随机数
```

#### 汇总数据

##### 聚集函数

```mysql
# avg()
select avg(prod_price) as avg_price from products where vend_id = 1993;
# 查询id为1993这个产品的价格平均值
# avg()函数忽略列值为null的行 空的会自动忽略不会被计算

# count()
select count(*) as num_cust from customers;
# 查询客户表的总条数
select count(cust_email) as num_cust from customers;
# 查询客户表中 邮箱不为null的总条数
# 如果使用count(*) 不会忽略null行

# max()
select max(prod_price) as max_price from products;
# 查询价格最贵的商品
select max(id) as max_row from products;
# 查询该表一共有多少行有效数据
# max() 忽略null值行

# min()
select min(prod_price) as min_price from products;
# 查询价格最便宜的商品
# min() 函数忽略null值行

# sum()
select sum(quantity) as items_ordered from orderitems where order_num = 20005;
# 查询orderitems 表中订单号为20005 的这条 一共有多少个商品
select sum(quantity * item_price) as total_price from orderitems where order_num = 2005;
# 查询订单号2005 的订单总价格
# sum() 函数忽略null值行
```

##### 组合聚集函数

```mysql
select count(*) as num_items,min(prod_price) as price_min,max(prod_price) as price_max,avg(prod_price) as price_avg from products;
# 查询产品表中的 总行数 价格最便宜的商品 价格最贵的商品 平均价格
# 使用函数直接计算 会比查询后在程序中计算会快得多
```

#### 分组数据

##### 创建分组

```mysql
select vend_id,count(*) as num_prods from products group by vend_id;
# 分组查询每个供应商制造的产品数量
# 如果分组中有null值，则null将作为一个分组返回，如果有多行null值，它们将分为一组
# group by 子句必须出现在where子句后面 order by 子句之前
# GROUP BY子句中列出的每个列都必须是检索列或有效的表达式
#（但不能是聚集函数）。如果在SELECT中使用表达式，则必须在
# GROUP BY子句中指定相同的表达式。不能使用别名。
```

##### 过滤分组

```mysql
select cust_id,count(*) as orders from orders group by cust_id having count(*) >=2;
select cust_id,count(*) as orders from orders group by cust_id having orders >=2;
# 查询每个下单的顾客的 订单数量 并且订单数量大于等于 2
# having 和 where一样 区别就是 where过滤分组前的数据 having过滤分组后结果

select count(*) as order_num,user_id from orders where order_status >= 10 group by user_id  having order_num >= 10 LIMIT 100;
# 查询 每个下单顾客 下单数量大于10单的 并且订单状态大于10的  前100条

select count(*) as order_num,user_id from orders where order_status >= 10 group by user_id  having order_num >= 10 order by order_num LIMIT 100;
# 查询 每个下单顾客 下单数量大于10单的 并且订单状态大于10的  前100条 并升序排列

select sum(goods_number * goods_price) as total,order_id from order_goods group by order_id  having total >= 1000 order by total desc LIMIT 100;
# 查询 每个订单的 总价的订单号 并且总价大于等于 1000 并且按总价排降序 的前一百条
```

##### 子句顺序

```
select --- from --- where --- group by --- having --- order by --- limit

```

#### 使用子查询

##### 子查询

```mysql
select email from user where id in();
select user_id from orders where id in();
select order_id from order_goods where goods_id = 8;

select email from user where id in(select user_id from orders where id in(select order_id from order_goods where goods_id = 8));
# 查询购买了产品id为8的的顾客姓名
```

##### 作为计算字段使用子查询

```mysql
select user_id,count(*) as order_num from orders  group by user_id order by order_num desc limit 100;
# 查询每个客户的订单总数

select email,(select count(*) from orders where orders.user_id = user.id) as order_num from user order by order_num desc;
# 查询每个客户的订单总数 和 邮箱
```

#### 联结表

##### 内部联结

```mysql
select orders.id,user.email from orders inner join user on user.id = orders.user_id;
# 使用内联查询下过订单的用户邮箱
# 只查询出符合关联条件的数据
```

##### 左联结

```mysql
select orders.id,user.email from orders left join user on user.id = orders.user_id;
# 左表为主表  查询主表的所有数据 如果 被关联的表符合关联条件就查出关联数据 没有就为空
# 也就是查询orders 表的全部数据 不管 user 是否有数据
```

##### 右联结

```mysql
select orders.id,user.email from orders right join user on user.id = orders.user_id;
# 右表为被关联的表  查询右表的所有数据 如果 被关联的表符合关联条件就查出关联数据 没有就为空
# 也就是查询user 表的全部数据 不管 order 是否有数据
```

#### 创建高级联结

##### 使用表别名

```mysql
select o.id,u.email from orders as o left join user as u on u.id = o.user_id;
```

##### 自联结

```mysql
# 想查询产品id等于 8的产品的供应商 查出该供应商产的全部产品
# 子查询方法
select prod_id,prod_name from products where vend_id = (select vend_id from products where prod_id = 8);
# 自联结方法
select prod_id,prod_name from products as p1,products as p2 where p1.vend_id = p2.vend_id and where p1.prod_id = 8;
```

##### 使用带聚集函数的联结

```mysql
select orders.id,user.email,count(orders.id) as order_num from orders inner join user on user.id = orders.user_id group by user.email order by  order_num desc;
# 使用聚集函数 + 内部联结 查询 每个下单用户的下单次数 并降序排列
```

#### 组合查询/复合查询

##### 使用union

```mysql
select order_sn from orders  where order_status >= 15;
select order_sn from orders  where user_id  = 19;
# 想查询订单状态大于 15 附加 下单用户id 等于 19的订单（不考虑价格）
select order_sn from orders  where order_status >= 15 union select orders_sn from orders  where user_id = 19;
#UNION必须由两条或两条以上的SELECT语句组成，语句之间用关
#键字UNION分隔（因此，如果组合4条SELECT语句，将要使用3个
#UNION关键字）。

# union 自动去除重复结果
# union all 
```

#### 全文本搜索

##### 要点

```mysql
MyISAM 数据库引擎支持全文本搜索
LIKE 和 正则匹配的搜索模式 性能极低
建表时需要指定全文查询的关键字段或之后指定 使用子句 FULLTEXT 将会对被指定的字段进行索引创建；MySQL会在新增、更改、删除行后自动维护该字段索引
```

##### 进行全文搜索

```mysql
select note_text from productnones where match(note_text) against('rabbit');
# match 指定索引字段
# against 指定要搜索的关键词
# 查找结果默认不区分大小写
# 使用全文搜索会根据查询数据的权重排序
```

##### 使用查询扩展

```mysql
# 作用：扩大查询范围即使查询的行数据中不包含指定关键词
# MySQL 工作流程
# 1.找出与搜索条件匹配的所有行
# 2.检查这些匹配行并选择所有有用的词
# 3.再次执行第一步 单搜索条件 增加了 第二部获取的关键词
select note_text from productnotes where match(note_text) against('anvils' with query expansion);
# 解析第二步 
# 假如搜索词为 php 搜索出的匹配行中有这样一条数据 i love php 
# 那么搜索结果可能会将"i love php"结果中的 love 作为扩展词
```

##### 布尔文本搜索

```mysql
select note_text from productnotes where match(note_text) against('heavy' in boolean mode);
# 优点不用设置 fulltext 索引
# 缺点非常慢

# 特殊规则查询
select note_text from productnotes where match(note_text) against('heavy -rope*' in boolean mode);
# 查询所有包含heavy 的结果 但处理以 “rope” 开头的 
```

##### 布尔全文搜索操作符

```mysql
# + 包含，关键词必须存在
# - 排除，关键词不允许出现
# > 包含，增加权重
# < 包含，减少权重
#（）表达式使用子表达式时
# ~ 取消一个词的排序值
# * 词尾的排序值
# "" 定义一个短语 eg: "i love you" 则必须包含i love you 才会被查询出来
```

##### 注意事项

```mysql
# 短词会被自动忽略 不会建立到索引中 短词长度为 <= 3
# 系统关键词自动忽略 
# 50% 如果一个词出现频率高于50% 则会忽略
# 如果表中的行数少于3行 则不会查询出任何结果 因为 50% 原则
# 忽略词中的单引号 如 don't 会被索引为 dont
# 日语和汉语 不能恰当的返回全文本搜索结果
# 只有MyISAM 引擎支持全文搜索
```

#### 插入数据

##### 数据插入

```mysql
insert into customers values(null,'weikai','100','angeles','ca','90046','usa');
# 仅指定要插入的表名 和 依次要插入的字段的值 
insert into customers(cust_name,cust_address,cust_city,cust_state) values('weikai','beijing','beijing','cn');
# 完整插入语句 指定插入的表名 和字段名 和字段值

```

##### 插入多个行

```mysql
insert into customers(cust_name,cust_address,cust_city,cust_state) values('weikai','beijing','beijing','cn');
insert into customers(cust_name,cust_address,cust_city,cust_state) values('weikai','beijing','beijing','cn');
insert into customers(cust_name,cust_address,cust_city,cust_state) values('weikai','beijing','beijing','cn');
#或者
insert into customers values(null,'weikai','100','angeles','ca','90046','usa'),(null,'weikai','100','angeles','ca','90046','usa'),
(null,'weikai','100','angeles','ca','90046','usa');

# for 循环插入数据可以使用一次插入多行 这样性能会比循环插入性能提高很多
```

##### 将查询出的数据插入到另一个表中

```mysql
insert into customers(cust_id,cust_contact,cust_email,cust_name) select cust_id,cust_contact,cust_email,cust_name from custnew;
# 查询custnew表中的所有行 并 将查询结果插入到customers 表中
# select 语句也可以使用where 来过滤查询结果
```

#### 更新和删除数据

##### 更新数据

```mysql
update  customers set cust_email = 'weikaiii@sina.cn' where cust_id='123';
# 修改用户表 用户id为123 的 邮箱为 weikaiii@sina.cn
# 关键词 update 表名 set 字段名 where 条件

update customers set cust_email = 'weikaiii@sina.cn',cust_name = 'weikai' where cust_id = '123';
# 同时修改满足条件的多个字段值
```

##### update 使用子查询

```mysql
update user u set email=(select email from admin where u.username = admin.username);
# 查询admin 表中用户名与user 表中一样的数据 修改他们的email
# 注意俩个字段的类型
```

##### 删除数据

```mysql
delete from customers where cust_id = 10005;
# 删除 customers 表 当cust_id 等于 10005的数据；
# 关键词 delete from 表名 where 条件
```

#### 创建和操控表

##### 创建表

```mysql
create table customers 
(
	cust_id int not null auto_increment,
    cust_name char(50) not null,
    cust_address char(50) null,
    primary key (cust_id)
) engine=Innodb;


# 关键词 create table 表名字 if not exists # 仅仅再表不存在时候创建防止报错
#（
#   字段名 字段类型 字段约束 ...
#        ...
#    primary key (主键字段名)
# ）engine=mysql 引擎

```

##### not null

```mysql
# 字段约束 not null 约束该字段值不能为空 如果赋予空值时会产生异常错误
```

##### 主键

```mysql
# 主键必须唯一
# 关键词 primary key (字段名)
# 主键自动应用规则 not null 
```

##### auto_increment

```mysql
# 自动增量 当插入新数据时自动维护字段值
# 关键词 auto_increment
# 每个表中只允许一个字段设置auto_increment 并且该字段必须被索引
```

##### last_insert_id

```mysql
# 该函数可获取最新插入的这条数据的 auto_increment 字段值
# select last_insert_id();
```

##### 指定默认值

```mysql
# 作用： 给指定字段当没被赋值时一个指定的默认值
# 关键字 default 123;
# 默认值仅支持 常量
```

##### 引擎类型

```mysql
# 关键词 engine= 引擎名
# InnoDB 可靠的事务处理引擎 不支持全文搜索
# MyISAM 高性能 支持全文搜索 不支持事务处理
# MEMORY 功能等同于MyISAM 数据储存再内存中 不在硬盘 速度快
# 外键不支持跨引擎
```

##### 更新表

```mysql
#关键词 alter table 表名 条件
alter table users add phone integer(11);
# 给已有的表 users 增加一个字段phone 
alter table users drop column phone;
# 给已有的表 users 删除一个字段 phone
alter table orderitems add constraint fk_orderitems_orders foreign key(order_num) references orders (order_num)
# 给表 orderitems 添加一个 外键 名为 fk_orderitems_orders 外键字段为 order_num 关联 orders 表 字段为order_num
```

##### 删除表

```mysql
drop table users;
```

##### 重命表

```mysql
rename table users to users;
```
