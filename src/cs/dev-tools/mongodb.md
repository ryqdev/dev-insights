# MongoDB basic

### How to install MongoDB

[https://www.mongodb.com/docs/manual/tutorial/install-mongodb-on-debian/](https://www.mongodb.com/docs/manual/tutorial/install-mongodb-on-debian/)

### How to start MongoDB

```shell
sudo systemctl start mongod
```

### How to operate MongoDB

* Show databases

```mongodb
show dbs
```

* Choose database

```mongodb
use <DATABASE_NAME>
```

* Get current databse

```mongodb
db
```

* Delete databse

```mongodb
db.dropDatabase()
```

* Show collections:

```mongodb
show collections
# or
db.getCollectionNames()
```

* delete collection

```mongodb
db.<collection_name>.drop()
```

* select \* from xxxx:

```mongodb
db.<collection_name>.find()
# or with more options
db.<collection_name>.find(query, projection).pretty()
```

### References

[https://www.runoob.com/mongodb/mongodb-dropdatabase.html](https://www.runoob.com/mongodb/mongodb-dropdatabase.html)

[https://computingforgeeks.com/how-to-install-mongodb-database-on-ubuntu/](https://computingforgeeks.com/how-to-install-mongodb-database-on-ubuntu/)
