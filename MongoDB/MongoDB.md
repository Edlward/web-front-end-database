
### MongoDB 的开启和关闭
	
	net start MongoDB -- 这是在已经配置好MongoDB的情况下载的
	net stop MongoDB

### 连接数据库
	
	直接输入 mongo 即可，也可以输入 mongo localhost:27017

### 相关命令

- show dbs

	查看有多少个库

- db 

	查看当前所在的库

- show collections / show tables

	查看有多少个集合，就是有多少个表

- db.c1.insert({name: 'jack', age: 28})

	隐式创建合集 c1 并插入数据

- db.createCollection('c3')

	显示创建合集 c3

- db.c3.drop()

	删除集合c3

- db.c1.find()

	查看集合c1中的数据

- use local

	切换到 local 库 或者创建数据库 local，当数据库不存在时创建。

- db.dropDatabase()

	删除当前数据库，在哪个库就删除哪个库 

- db.help() / db.c1.help()

	查看mongodb 的命令 或者db.c1下能用的命令

- db.c1.find({}).count()

	查看有多少条数据

- db.c1.remove({}) / db.c1.remove() 

	删除c1中的所有数据，但是不会删除表/集合c1

- db.c1.remove({name: 'jack'})

	删除c1中的 name: 'jack'数据

- db.集合名称.find({条件})

	查询集合如： db.c1.find({name: 'jack'}) 查询 c1 中 name 为 jack的数据

- db.c1.findOne()

	取出集合中的第一条数据

- db.集合名称.find({查询条件}, {要显示的key属性})

	db.c1.find({}, {name: 1}) -- 查询c1所有数据，只显示name字段
	默认 _id 键值是显示的，如果不想显示_id，则让_id=0
		db.c1.find({}, {name: 1, _id: 0})	

	db.c1.find({}, {name: 0}) -- 查询c1所有数据，不显示name字段，其他的字段都显示

- 查询集合中的文档 ，使用条件表达式(<, <=, >, >=,!=)

	//大于： field > value
	db.collection.find({field:{$gt:value}});
	
	//小于： field < value
	db.collection.find({field:{$lt:value}});
	
	//大于等于： field >= value
	db.collection.find({field:{$gte:value}});
	
	//小于等于： field <= value
	db.collection.find({field:{$lte:value}});
	
	//不等于：  field != value
	db.collection.find({field:{$ne:value}});

	// 等于
	db.c2.find({age: 25})

- 查询集合中的文档 ，统计(count)、排序(sort)、分页(skip、limit)
	
	// 查表中有多少条数据
	db.customer.count();
	db.customer.find().count();

	// 查表中 age小于5的有多少条数据
	db.customer.find({age:{$lt:5}}).count();

	// 查表中有多少条数据，然后按age进行排序
	db.customer.find().sort({age:1});

	// 查表中有多少条数据，然后跳过前2条，并限制只显示3条
	db.customer.find().skip(2).limit(3);

	// 查表中有多少条数据，倒序显示，然后跳过前2条，并限制只显示3条
	db.c2.find().sort({age:-1}).skip(2).limit(3);

	// 查表中有多少条数据，count()内没有数字，就是统计find()中找到的个数
	db.c2.find().sort({age:-1}).skip(2).limit(3).count();
	db.c2.find().sort({age:-1}).skip(2).limit(3).count(0);

	// 查表中有多少条数据，因为count(1)所以会显示输出的数据条数，为 3 条
	db.c2.find().sort({age:-1}).skip(2).limit(3).count(1);


- $all 用来查询数据，中是否包涵所有的数据，只要有一条不包涵就不会显示输出

	db.c2.find({order: {$all: [1, 2, 5]}})
	这条查询要求order数组中必须包涵有 1， 2 ，5 的才会补显示输出，只要有一个不包涵如order:[1, 3, 5] 则不会被找到。

- $in 也是用来查询数组，但是只要包涵有一个就会被显示输出


	db.c2.find({order: {$in: [1, 2, 5]}})
	只要order中包涵有1， 2， 5 中的一个就 会 被显示输出
	
- $nin 和 $in相反 只要包涵其中一个就不显示

	db.c2.find({order: {$in: [1, 2, 5]}})
	只要order中包涵有1， 2， 5 中的一个就 不会 被显示输出

- $or 相当于关系型数据库中的OR，表示或者的关系

	例如查询name为user2或者age为3的文档：
	db.customer.find({$or:[{name:”user2”},{age:3}]})
	注意 $or 的位置。

- $nor，表示根据条件过滤掉某些数据

	例如查询name不是user2，age不是3的文档，命令为：
	db.customer.find({$nor:[{name:”user2”},{age:3}]})

- $exists，用于查询集合中存在某个键的文档或不存在某个键的文档

	$exists:1表示真，指存在
	$exists:0表示假，指不存在
	如：
	db.c2.find({order: {$exists: 1}}) 表示查找 存在 order字段的集合并显示输出
	db.c2.find({order: {$exists: 0}}) 表示查找 不存在 order字段的集合并显示输出

- 类似于关系型数据库，mongodb中也有游标的概念
	
	var x = db.c2.find();  // x 中存在查找的所有数据
	x.hasNext()  --> true/false  表示是否有下一条数据
	x.next()  输出下一条数据，x中就会减少一条数据

- 更新集合中的文档

	db.collection.update(criteria,objNew,upsert,multi)

	参数说明：
	criteria:用于设置查询条件的对象
	objNew:用于设置更新内容的对象
	upsert:如果记录已经存在，更新它，否则新增一个记录，取值为0或1
			0--表示找到要更新的值，就更新对应设置，没找到就不更新
			1--表示找到更新，没找到则新增加一条数据
	multi：如果有多个符合条件的记录，是否全部更新，取值为0或1
			0--只更新第一条
			1--更新找到的全部数据 这个时候要在更新的数据中加入$set
	
	注意：默认情况下，只会更新第一个符合条件的记录
	一般情况下后两个参数分别为0,1 ，即：
	db.collection.update(criteria,objNew,0,1)
	如：db.c2.update({name: "jack"}, {$set: {age: 26}}, 0, 1)
		表示更新name为jack的集合中age为26其他的不变
	或者：db.c2.update({name:"richard"},{$set:{age:28}},0,1)
	
	- $set 设置对应属性的新值，不改变其他的属性，用来指定一个键的值，如果这个键不存在，则创建它
	
		最好都加$set不然，会将要更新的集合，设置为只要新更新的数据，旧的会被覆盖。
		db.c2.update({name:"richard"},{$set:{age:28}},0,1)

	- $inc 表示增加
		
		如：db.c2.update({name:"richard"},{$inc:{age:1}},0,1)
			表示将age 加 1，如果name:"richard"中 没有age这个属性，则将age设置为 1
	
		db.c2.update({name:"richard"},{$inc:{age: -1}},0,1)
			表示将 age 减 1

	- $unset 用来删除某个键
	
		例如删除name为user1的文档中的address键，可以使用命令：
			db.c1.update({name:”user1”},{$unset:{address:1}},0,1)















