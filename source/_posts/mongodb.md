---
title: mongodb学习
date: 2019-01-29
tags: [mongodb,java,maven]
categories: mongodb
---

[TOC]

### SQL与NOSQL简介

- ​SQL

  关系模型适合于客户服务器编程，远远超出预期利益

  关系型数据库遵循A(原子性) C(一致性) I(独立性) D(持久性)

- NOSQL
  非关系型数据库，用于超大规模数据的存储，无需多余操作就可以横向扩展

  关系模型适合于客户服务器编程，远远超出预期利益

### MongoDB

#### 简介

基于分布式文件存储,为WEB应用提供可扩展的高性能数据存储解决方案

MongoDB将数据存储为一个文档，数据结构由键值(key=>value)对组成。文档类似于JSON对象。

1. MongoDB区分类型和大小写
2. MongoDB文档不能有重复的键
3. 文档中键值对是有序的
4. 文档中值不仅可以使字符串，还介意是其他数据类型
5. 文档键是字符串，可使用UTF-8字符

**注意**:键不能含有\0空字符，$不能使用，_开头的键是保留的	

#### 数据类型

String、Integer、Boolean、Double、

Min/Max keys  将一个值与BSON(二进制的json)元素最低值和最高值对比、

Array、Timestramp、Object、Null、

Symbol(符号)、Date、Object ID、Binary Data、Code(代码类型)、Regular expression(正则表达式)

#### 常见密令

启动：net start mongoDB

关闭：net stop mongoDB

后台管理shell： mongo

查看数据库：show dbs

进入数据库：use dbname

查看当前所属数据库：db

删除数据库：db.dropDatabase()

创建集合：db.createCollection(name,options) mongodb不需要创建集合，在插入文档时，会自动创建集合

删除集合：db.collectionname.drop()

插入文档：

1. db.collectionname.insert(document)

   ex：

   db.color.insert({red:'1',green:'2',blue:'3',purple:'4',gray:'5',pink:'6',orange:'7',yellow:'8',white:'9',black:'10'}) WriteResult({ "nInserted" : 1 })

2. 将数据定义为一个变量document，然后执行插入操作db.collectionname.insert(document)

   ex:  

   document=({title: 'MongoDB 教程', description: 'MongoDB 是一个 Nosql 数据库',by: '菜鸟教程',url: 'http://www.runoob.com',tags: ['mongodb', 'database', 'NoSQL'],likes: 100});	
   		db.cainiao.insert(document)

	
			ex:  db.color.insert({red:'1',green:'2',blue:'3',purple:'4',gray:'5',pink:'6',orange:'7',yellow:'8',white:'9',black:'10'}) WriteResult({ "nInserted" : 1 })
			 
			
	查看文档内容：db.collectionname.find()   会水平输出
	更新文档：1.db.collectionname.update(
					<query>,
   					<update>,
   					{
     					upsert: <boolean>,
     					multi: <boolean>,
     					writeConcern: <document>
   					}
   			    )
		query : update的查询条件，类似sql update查询内where后面的。
		update : update的对象和一些更新的操作符（如$,$inc...）等，也可以理解为sql update查询内set后面的
		upsert : 可选，这个参数的意思是，如果不存在update的记录，是否插入objNew,true为插入，默认是false，不插入。
		multi : 可选，mongodb 默认是false,只更新找到的第一条记录，如果这个参数为true,就把按条件查出来多条记录全部更新。
		writeConcern :可选，抛出异常的级别

		ex: db.cainiao.update({'title':'MongoDB 教程'},{$set:{'title':'MongoDB'}})
			查看文档：db.cainiao.find().pretty()   会竖直输出
			 2. db.collectionname.save(
			 		<document>,
				   {
				       writeConcern: <document>
				   }
			    ) 
	    document : 文档数据。
		writeConcern :可选，抛出异常的级别。
	
		ex： db.cainiao.save(document) 然后查看文档
	
		删改某一条数据
		db.getCollection('system_menu').update({"name":"蜻蜓列表"},{$set:{path:"fm/list?type=album"}})
	删除文档： remove() 在执行remove之前先find判断执行的条件是否正确
				db.collection.remove(
				   <query>,
				   {
				     justOne: <boolean>,
				     writeConcern: <document>
				   }
				)
		query :（可选）删除的文档的条件。
		justOne : （可选）如果设为 true 或 1，则只删除一个文档。
		writeConcern :（可选）抛出异常的级别
		ex:	db.cainiao.insert(document)
			db.cainiao.insert({title: 'MongoDB 教程', description: 'MongoDB 是一个 Nosql 数据库',by: '菜鸟教程',url: 'http://www.runoob.com',tags: ['mongodb', 'database', 'NoSQL'],likes: 100})
			db.cainiao.remove({'title':'MongoDB 教程'})   删除全部
			db.cainiao.remove({'title':'MongoDB 教程'},1) 删除一条
	查询文档： find()
				db.collection.find(query, projection)
		query ：可选，使用查询操作符指定查询条件
		projection ：可选，使用投影操作符指定返回的键。查询时返回文档中所有键值， 只需省略该参数即可（默认省略）。
	
		易读取数据，可以使用 pretty()方法,pretty()以格式化的方式来显示所有文档。
	
		除了find()方法还有findOne()方法，只返回一个文档
	
		条件语句查询：
			等于 {<key>:<value>} 
			小于 {<key>:{$lt:<value>}}
			大于 {<key>:{$gt:<value>}}
			小于等于 {<key>:{$lte:<value>}}
			大于等于 {<key>:{$gte:<value>}}
			不等于 {<key>:{$ne:<value>}}
	
			AND条件 每个键以逗号隔开
		ex：db.cainiao.find({"by":"菜鸟教程", "title":"MongoDB 教程"}).pretty()
			OR条件  使用关键字$or
		ex：db.cainiao.find({$or:[{"by":"菜鸟教程"},{"title": "MongoDB 教程"}]}).pretty()
			AND与OR联合使用
		ex：db.cainiao.find({"likes": {$gt:50}, $or: [{"by": "菜鸟教程"},{"title": "MongoDB 教程"}]}).pretty()
	
	$type操作符
		$type是基于BSON类型来检索集合中匹配的数据类型，并返回结果
		ex： 获取title为字符串String类型的数据
		db.col.find({"title" : {$type : 2}})
		或
		db.col.find({"title" : {$type : 'string'}})
	
	Limit与Skip方法
		limit：db.collectionname.find().limit(number)
 		ex：获取title这一列数据两条
 		db.col.find({},{"title":1,_id:0}).limit(2)  
		skip：db.collectionname.find().limit(number).skip(number)
		使用skip()方法来跳过指定数量的数据
	Sort()方法
		db.collectionname.find().sort({KEY:1})
		ex：db.col.find({},{"title":1,_id:0}).sort({"likes":-1}) 按降序排列

	索引
		创建索引：
		createIndex()方法
		db.collection.createIndex(keys, options)
		keys：值为你要创建的索引字段
		options：1为指定按升序创建，-1按降序来创建
		重建索引：
		reIndex()方法
		db.collectionname.reIndex()
		查看索引：
		getIndexes()方法
		db.collectionname.getIndexes()
		删除索引：
		dropIndex()方法
		db.collectionname.dropIndex("indexname")
	聚合
		aggregate()方法
		ex：db.col.aggregate([{$group:{_id:"$title",num:{$sum:1}}}])
		聚合表达式：
			$sum    	计算综合
			$avg		计算平均值	
			$min		获取最小值	
			$max 		获取最大值
			$push 		将结果文档插入到一个数组中
			$addToSet	将结果文档插入到数组中，但不创建副本
			$first 		根据排序获取第一个数据
			$last 		获取最后一个数据
		管道：
		在linux和unix一般用于将当前命令的输出结果座位下一个命令的参数
		MongoDB的聚合管道将MongoDB文档在一个管待处理完毕后将结果传递给下一个管道，可重复操作
		MongoDB常用管道操作：
			$project 	修改输入文档的结构
			$match 		过滤数据，输出符合条件的文档
			$limit 		限制聚合管道返回文档数
			$skip 		在聚合管道中跳过指定数量文档
			$unwind 	将文档中某一数组类型字段拆分成多条，每条包含数组中一个值
			$group 		将集合中文档分组，便于统计
			$sort 		将输入文档排序后输出
			$geoNear 	输出接近某一地理位置的有序文档
		ex：db.col.aggregate({$project:{title:1,by:1,_id:0}}) 输出title，by两列
	
			db.col.aggregate([{$match:{likes:{$gt:120,$lte:200}}},{$group:{_id:"$title",count:{$sum:1}}}]) 输出likes大于120，小于等于200的以title分组的个数
	
			db.col.aggregate([{$skip:1},{$limit:1}]) 输出跳过一条数据然后输出一条