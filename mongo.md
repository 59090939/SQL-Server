https://blog.csdn.net/Supreme_Sir/article/details/114959912
https://blog.csdn.net/zhuchunyan_aijia/article/details/93618862
https://www.cnblogs.com/hushaojie/p/12132365.html
https://www.cnblogs.com/kevingrace/p/5685457.html
https://blog.csdn.net/h952520296/article/details/112478369
https://fastdl.mongodb.org/linux/mongodb-linux-x86_64-rhel70-4.4.4.tgz
https://fastdl.mongodb.org/linux/mongodb-linux-x86_64-rhel70-7.0.15.tgz

https://stackoverflow.com/questions/21979544/mongodb-not-sharding
https://dba.stackexchange.com/questions/300706/not-able-to-shard-collection


db.runCommand( { enablesharding :"testdb"});
db.runCommand( { shardcollection : "testdb.books_range", key : { id : 1 } } );
use testdb
db.stats();
for (var i = 1; i <= 20000; i++) db.books_range.insert({id:i,name:"12345678",sex:"male",age:27,value:"test"});


db.runCommand( { shardcollection : "testdb.books_hash", key : { id : "hashed" } } );
sh.shardCollection("testdb.books_hash",{"id": "hashed"})
use testdb
for (var i = 1; i <= 20000; i++) db.books_hash.save({id:i,name:"12345678",sex:"male",age:27,value:"test"});

for(var i=1;i<= 1000;i++){
    db.sir.insert({"name":"test"+i,
    salary:(Math.random()*20000).toFixed(2)});
}
