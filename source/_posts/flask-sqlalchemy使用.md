---
title: flask-sqlalchemy使用
date: 2023-01-12 15:48:11
tags:
---

## 查询

```js
1.  filter()    把过滤器添加到原查询上，返回一个新查询

2.  filter_by()    把等值过滤器添加到原查询上，返回一个新查询

3.  limit    使用指定的值限定原查询返回的结果

4.  offset()    偏移原查询返回的结果，返回一个新查询

5.  order_by()    根据指定条件对原查询结果进行排序，返回一个新查询

6.  group_by()    根据指定条件对原查询结果进行分组，返回一个新查询

7.  all()    以列表形式返回查询的所有结果

8.  first()    返回查询的第一个结果，如果未查到，返回 None

9.  first_or_404()    返回查询的第一个结果，如果未查到，返回 404

10.  get()    返回指定主键对应的行，如不存在，返回 None

11.  get_or_404()    返回指定主键对应的行，如不存在，返回 404

12.  count()    返回查询结果的数量

13.  paginate()    返回一个Paginate对象，它包含指定范围内的结果
```

```js
1.  """

2.  查询所有用户数据

3.  User.query.all()

5.  查询有多少个用户

6.  User.query.count()

8.  查询第1个用户

9.  User.query.first()

10.  User.query.get(1) # 根据id查询

12.  查询id为4的用户[3种方式]

13.  User.query.get(4)

14.  User.query.filter_by(id=4).all() # 简单查询 使用关键字实参的形式来设置字段名

15.  User.query.filter(User.id == 4).all() # 复杂查询 使用恒等式等其他形式来设置条件

17.  查询名字结尾字符为g的所有用户[开始 / 包含]

18.  User.query.filter(User.name.endswith("g")).all()

19.  User.query.filter(User.name.startswith("w")).all()

20.  User.query.filter(User.name.contains("n")).all()

21.  User.query.filter(User.name.like("%n%g")).all() 模糊查询

23.  查询名字和邮箱都以li开头的所有用户[2种方式]

24.  User.query.filter(User.name.startswith("li"), User.email.startswith("li")).all()

26.  from sqlalchemy import and_

27.  User.query.filter(and_(User.name.startswith("li"), User.email.startswith("li"))).all()

29.  查询age是25 或者 \`email\`以\`itheima.com\`结尾的所有用户

30.  from sqlalchemy import or_

31.  User.query.filter(or_(User.age == 25, User.email.endswith("itheima.com"))).all()

33.  查询名字不等于wang的所有用户[2种方式]

34.  from sqlalchemy import not_

35.  User.query.filter(not_(User.name == "wang")).all()

36.  User.query.filter(User.name != "wang").all()

38.  查询id为[1, 3, 5, 7, 9]的用户

39.  User.query.filter(User.id.in_([1, 3, 5, 7, 9])).all()

41.  所有用户先按年龄从小到大, 再按id从大到小排序, 取前5个

42.  User.query.order_by(User.age, User.id.desc()).limit(5).all()

44.  分页查询, 每页3个, 查询第2页的数据

45.  pn = User.query.paginate(2, 3)

46.  pn.items 获取该页的数据 pn.page 获取当前的页码 pn.pages 获取总页数
```
