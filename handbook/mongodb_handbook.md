# [Installation](https://docs.mongodb.com/manual/installation/)

## [Install MongoDB Community Edition](https://docs.mongodb.com/manual/administration/install-community/)

### [Install on Linux](https://docs.mongodb.com/manual/administration/install-on-linux/)

#### [Install on Ubuntu](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-ubuntu/)

**Import the public key used by the package management system.**

```
sudo apt-get install gnupg
wget -qO - https://www.mongodb.org/static/pgp/server-4.2.asc | sudo apt-key add -
```

**Create a list file for MongoDB**

Create the `/etc/apt/sources.list.d/mongodb-org-4.2.list` file for Ubuntu 18.04 (Bionic):

```
echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu bionic/mongodb-org/4.2 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.2.list
```

Create the `/etc/apt/sources.list.d/mongodb-org-4.2.list` file for Ubuntu 16.04 (Xenial):

```
echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu xenial/mongodb-org/4.2 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.2.list
```

**Reload local package database.**

```
sudo apt-get update
```

**Install the MongoDB packages**

To install the latest stable version, issue the following

```
sudo apt-get install -y mongodb-org
```

To install a specific release, you must specify each component package individually along with the version number, as in the following example:

```
sudo apt-get install -y mongodb-org=4.2.8 mongodb-org-server=4.2.8 mongodb-org-shell=4.2.8 mongodb-org-mongos=4.2.8 mongodb-org-tools=4.2.8
```

To prevent unintended upgrades, you can pin the package at the currently installed version:

```
echo "mongodb-org hold" | sudo dpkg --set-selections
echo "mongodb-org-server hold" | sudo dpkg --set-selections
echo "mongodb-org-shell hold" | sudo dpkg --set-selections
echo "mongodb-org-mongos hold" | sudo dpkg --set-selections
echo "mongodb-org-tools hold" | sudo dpkg --set-selections
```

**Init System**

To run and manage your [`mongod`](https://docs.mongodb.com/manual/reference/program/mongod/#bin.mongod) process, you will be using your operating system’s built-in [init system](https://docs.mongodb.com/manual/reference/glossary/#term-init-system). Recent versions of Linux tend to use **systemd** (which uses the `systemctl` command), while older versions of Linux tend to use **System V init** (which uses the `service` command).

If you are unsure which init system your platform uses, run the following command:

```
ps --no-headers -o comm 1
```

```
sudo systemctl start mongod
sudo systemctl status mongod
sudo systemctl enable mongod
sudo systemctl stop mongod
sudo systemctl restart mongod
mongo
```

**Uninstall MongoDB Community Edition**

```
sudo service mongod stop
sudo apt-get purge mongodb-org*
sudo rm -r /var/log/mongodb
sudo rm -r /var/lib/mongodb
```

# [The mongo Shell](https://docs.mongodb.com/manual/mongo/)

```
mongo
mongo --port 27017
mongo "mongodb://mongodb0.example.com:28015"
mongo --host mongodb0.example.com:28015
mongo --host mongodb0.example.com --port 28015
mongo "mongodb://alice@mongodb0.examples.com:28015/?authSource=admin"
mongo --username alice --password --authenticationDatabase admin --host mongodb0.examples.com --port 28015
mongo "mongodb://mongodb0.example.com.local:27017,mongodb1.example.com.local:27017,mongodb2.example.com.local:27017/?replicaSet=replA"
mongo --host replA/mongodb0.example.com.local:27017,mongodb1.example.com.local:27017,mongodb2.example.com.local:27017
mongo "mongodb://mongodb0.example.com.local:27017,mongodb1.example.com.local:27017,mongodb2.example.com.local:27017/?replicaSet=replA&ssl=true"
mongo "mongodb+srv://server.example.com/"
mongo --ssl --host replA/mongodb0.example.com.local:27017,mongodb1.example.com.local:27017,mongodb2.example.com.local:27017
```

## Working with the `mongo` Shell[¶](https://docs.mongodb.com/manual/mongo/#working-with-the-mongo-shell)

```
mongo --help
db
help
use test
show dbs
quit()
```

## Collection Help[¶](https://docs.mongodb.com/manual/tutorial/access-mongo-shell-help/#collection-help)

```
show collections
db.collection.save
```

# [MongoDB CRUD Operations](https://docs.mongodb.com/manual/crud/)

## [Insert Documents](https://docs.mongodb.com/manual/tutorial/insert-documents/#insert-documents)

Insert a Single Document[¶](https://docs.mongodb.com/manual/tutorial/insert-documents/#insert-a-single-document)

```
db.inventory.insertOne(
   { item: "canvas", qty: 100, tags: ["cotton"], size: { h: 28, w: 35.5, uom: "cm" } }
)
db.inventory.find( { item: "canvas" } )
```

Insert Multiple Documents[¶](https://docs.mongodb.com/manual/tutorial/insert-documents/#insert-multiple-documents)

```
db.inventory.insertMany([
   { item: "journal", qty: 25, tags: ["blank", "red"], size: { h: 14, w: 21, uom: "cm" } },
   { item: "mat", qty: 85, tags: ["gray"], size: { h: 27.9, w: 35.5, uom: "cm" } },
   { item: "mousepad", qty: 25, tags: ["gel", "blue"], size: { h: 19, w: 22.85, uom: "cm" } }
])
db.inventory.find( {} )
```

### [Insert Methods](https://docs.mongodb.com/manual/reference/insert-methods/#insert-methods)

MongoDB provides the following methods for inserting [documents](https://docs.mongodb.com/manual/core/document/#bson-document-format) into a collection:

| [`db.collection.insertOne()`](https://docs.mongodb.com/manual/reference/method/db.collection.insertOne/#db.collection.insertOne) | Inserts a single document into a collection.                 |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [`db.collection.insertMany()`](https://docs.mongodb.com/manual/reference/method/db.collection.insertMany/#db.collection.insertMany) | [`db.collection.insertMany()`](https://docs.mongodb.com/manual/reference/method/db.collection.insertMany/#db.collection.insertMany) inserts *multiple* [documents](https://docs.mongodb.com/manual/core/document/#bson-document-format) into a collection. |
| [`db.collection.insert()`](https://docs.mongodb.com/manual/reference/method/db.collection.insert/#db.collection.insert) | [`db.collection.insert()`](https://docs.mongodb.com/manual/reference/method/db.collection.insert/#db.collection.insert) inserts a single document or multiple documents into a collection. |

Additional Methods for Inserts[¶](https://docs.mongodb.com/manual/reference/insert-methods/#additional-methods-for-inserts)

The following methods can also add new documents to a collection:

- [`db.collection.update()`](https://docs.mongodb.com/manual/reference/method/db.collection.update/#db.collection.update) when used with the `upsert: true` option.
- [`db.collection.updateOne()`](https://docs.mongodb.com/manual/reference/method/db.collection.updateOne/#db.collection.updateOne) when used with the `upsert: true` option.
- [`db.collection.updateMany()`](https://docs.mongodb.com/manual/reference/method/db.collection.updateMany/#db.collection.updateMany) when used with the `upsert: true` option.
- [`db.collection.findAndModify()`](https://docs.mongodb.com/manual/reference/method/db.collection.findAndModify/#db.collection.findAndModify) when used with the `upsert: true` option.
- [`db.collection.findOneAndUpdate()`](https://docs.mongodb.com/manual/reference/method/db.collection.findOneAndUpdate/#db.collection.findOneAndUpdate) when used with the `upsert: true` option.
- [`db.collection.findOneAndReplace()`](https://docs.mongodb.com/manual/reference/method/db.collection.findOneAndReplace/#db.collection.findOneAndReplace) when used with the `upsert: true` option.
- [`db.collection.save()`](https://docs.mongodb.com/manual/reference/method/db.collection.save/#db.collection.save).
- [`db.collection.bulkWrite()`](https://docs.mongodb.com/manual/reference/method/db.collection.bulkWrite/#db.collection.bulkWrite).

## [Query Documents](https://docs.mongodb.com/manual/tutorial/query-documents/#query-documents)

Select All Documents in a Collection[¶](https://docs.mongodb.com/manual/tutorial/query-documents/#select-all-documents-in-a-collection)

```
db.inventory.find( {} )
```

Specify Equality Condition[¶](https://docs.mongodb.com/manual/tutorial/query-documents/#specify-equality-condition)

To specify equality conditions, use `<field>:<value>` expressions in the [query filter document](https://docs.mongodb.com/manual/core/document/#document-query-filter):

```
{ <field1>: <value1>, ... }
```

The following example selects from the `inventory` collection all documents where the `status` equals `"D"`:

```
db.inventory.find( { status: "D" } )
```

Specify Conditions Using Query Operators[¶](https://docs.mongodb.com/manual/tutorial/query-documents/#specify-conditions-using-query-operators)

A [query filter document](https://docs.mongodb.com/manual/core/document/#document-query-filter) can use the [query operators](https://docs.mongodb.com/manual/reference/operator/query/#query-selectors) to specify conditions in the following form:

```
{ <field1>: { <operator1>: <value1> }, ... }
```

The following example retrieves all documents from the `inventory` collection where `status` equals either `"A"` or `"D"`:

```
db.inventory.find( { status: { $in: [ "A", "D" ] } } )
```

Specify `AND` Conditions[¶](https://docs.mongodb.com/manual/tutorial/query-documents/#specify-and-conditions)

```
db.inventory.find( { status: "A", qty: { $lt: 30 } } )
```

Specify `OR` Conditions[¶](https://docs.mongodb.com/manual/tutorial/query-documents/#specify-or-conditions)

```
db.inventory.find( { $or: [ { status: "A" }, { qty: { $lt: 30 } } ] } )
```

Specify `AND` as well as `OR` Conditions[¶](https://docs.mongodb.com/manual/tutorial/query-documents/#specify-and-as-well-as-or-conditions)

```
db.inventory.find( {
     status: "A",
     $or: [ { qty: { $lt: 30 } }, { item: /^p/ } ]
} )
```

**NOTE**

MongoDB supports regular expressions [`$regex`](https://docs.mongodb.com/manual/reference/operator/query/regex/#op._S_regex) queries to perform string pattern matches.

### [Query on Embedded/Nested Documents](https://docs.mongodb.com/manual/tutorial/query-embedded-documents/)

Match an Embedded/Nested Document[¶](https://docs.mongodb.com/manual/tutorial/query-embedded-documents/#match-an-embedded-nested-document)

```
db.inventory.find( { size: { h: 14, w: 21, uom: "cm" } } )
```

Query on Nested Field[¶](https://docs.mongodb.com/manual/tutorial/query-embedded-documents/#query-on-nested-field)

To specify a query condition on fields in an embedded/nested document, use [dot notation](https://docs.mongodb.com/manual/reference/glossary/#term-dot-notation) (`"field.nestedField"`).

**NOTE**
When querying using dot notation, **the field and nested field must be inside quotation marks.**

```
db.inventory.find( { "size.uom": "in" } )
```

Specify Match using Query Operator[¶](https://docs.mongodb.com/manual/tutorial/query-embedded-documents/#specify-match-using-query-operator)

```
db.inventory.find( { "size.h": { $lt: 15 } } )
```

Specify `AND` Condition[¶](https://docs.mongodb.com/manual/tutorial/query-embedded-documents/#specify-and-condition)

```
db.inventory.find( { "size.h": { $lt: 15 }, "size.uom": "in", status: "D" } )
```

### [Query an Array](https://docs.mongodb.com/manual/tutorial/query-arrays/)

```
db.inventory.insertMany([
   { item: "journal", qty: 25, tags: ["blank", "red"], dim_cm: [ 14, 21 ] },
   { item: "notebook", qty: 50, tags: ["red", "blank"], dim_cm: [ 14, 21 ] },
   { item: "paper", qty: 100, tags: ["red", "blank", "plain"], dim_cm: [ 14, 21 ] },
   { item: "planner", qty: 75, tags: ["blank", "red"], dim_cm: [ 22.85, 30 ] },
   { item: "postcard", qty: 45, tags: ["blue"], dim_cm: [ 10, 15.25 ] }
]);
```

Match an Array[¶](https://docs.mongodb.com/manual/tutorial/query-arrays/#match-an-array)

To specify equality condition on an array, use the query document `{ <field>: <value> }` where `<value>` is the exact array to match, including the order of the elements.

```
db.inventory.find( { tags: ["red", "blank"] } )
db.inventory.find( { tags: { $all: ["red", "blank"] } } )
```

Query an Array for an Element[¶](https://docs.mongodb.com/manual/tutorial/query-arrays/#query-an-array-for-an-element)

The following example queries for all documents where `tags` is an array that contains the string `"red"` as one of its elements:

```
db.inventory.find( { tags: "red" } )
```

For example, the following operation queries for all documents where the array `dim_cm` contains at least one element whose value is greater than `25`.

```
db.inventory.find( { dim_cm: { $gt: 25 } } )
```

**Specify Multiple Conditions for Array Elements[¶](https://docs.mongodb.com/manual/tutorial/query-arrays/#specify-multiple-conditions-for-array-elements)**

Query an Array with Compound Filter Conditions on the Array Elements

The following example queries for documents where the `dim_cm` array contains elements that in some combination satisfy the query conditions; e.g., one element can satisfy the greater than `15` condition and another element can satisfy the less than `20` condition, or a single element can satisfy both:

```
db.inventory.find( { dim_cm: { $gt: 15, $lt: 20 } } )
```

Query for an Array Element that Meets Multiple Criteria[¶](https://docs.mongodb.com/manual/tutorial/query-arrays/#query-for-an-array-element-that-meets-multiple-criteria)

Use [`$elemMatch`](https://docs.mongodb.com/manual/reference/operator/query/elemMatch/#op._S_elemMatch) operator to specify multiple criteria on the elements of an array such that at least one array element satisfies all the specified criteria.

The following example queries for documents where the `dim_cm` array contains at least one element that is both greater than ([`$gt`](https://docs.mongodb.com/manual/reference/operator/query/gt/#op._S_gt)) `22` and less than ([`$lt`](https://docs.mongodb.com/manual/reference/operator/query/lt/#op._S_lt)) `30`:

```
db.inventory.find( { dim_cm: { $elemMatch: { $gt: 22, $lt: 30 } } } )
```

Query for an Element by the Array Index Position[¶](https://docs.mongodb.com/manual/tutorial/query-arrays/#query-for-an-element-by-the-array-index-position)

The following example queries for all documents where the second element in the array `dim_cm` is greater than `25`:

```
db.inventory.find( { "dim_cm.1": { $gt: 25 } } )
```

Query an Array by Array Length[¶](https://docs.mongodb.com/manual/tutorial/query-arrays/#query-an-array-by-array-length)

```
db.inventory.find( { "tags": { $size: 3 } } )
```

### [Query an Array of Embedded Documents](https://docs.mongodb.com/manual/tutorial/query-array-of-documents/)



### [Project Fields to Return from Query](https://docs.mongodb.com/manual/tutorial/project-fields-from-query-results/)



### [Query for Null or Missing Fields](https://docs.mongodb.com/manual/tutorial/query-for-null-fields/)

# [Security](https://docs.mongodb.com/manual/security/#security)

## [Enable Access Control](https://docs.mongodb.com/manual/tutorial/enable-authentication/)

Start MongoDB without access control.[¶](https://docs.mongodb.com/manual/tutorial/enable-authentication/#start-mongodb-without-access-control)

```
mongod --port 27017 --dbpath /var/lib/mongodb
```

Connect to the instance.[¶](https://docs.mongodb.com/manual/tutorial/enable-authentication/#connect-to-the-instance)

```
mongo --port 27017
```

Create the user administrator.[¶](https://docs.mongodb.com/manual/tutorial/enable-authentication/#create-the-user-administrator)

```
use admin
db.createUser(
  {
    user: "myUserAdmin",
    pwd: passwordPrompt(), // or cleartext password
    roles: [ { role: "userAdminAnyDatabase", db: "admin" }, "readWriteAnyDatabase" ]
  }
)
```

Re-start the MongoDB instance with access control.[¶](https://docs.mongodb.com/manual/tutorial/enable-authentication/#re-start-the-mongodb-instance-with-access-control)

```
sudo vi /etc/mongod.conf
```

add

```
security:
    authorization: enabled
```

Restart

```
sudo systemctl restart mongod
```

Or

```
db.adminCommand( { shutdown: 1 } )
mongod --auth --port 27017 --dbpath /var/lib/mongodb
```

Connect and authenticate as the user administrator.[¶](https://docs.mongodb.com/manual/tutorial/enable-authentication/#connect-and-authenticate-as-the-user-administrator)

```
mongo --port 27017
use admin
db.auth("myUserAdmin", passwordPrompt()) // or cleartext password
```

```
mongo --port 27017  --authenticationDatabase "admin" -u "myUserAdmin" -p
```

Create additional users as needed for your deployment.[¶](https://docs.mongodb.com/manual/tutorial/enable-authentication/#create-additional-users-as-needed-for-your-deployment)

```
use test
db.createUser(
  {
    user: "myTester",
    pwd:  passwordPrompt(),   // or cleartext password
    roles: [ { role: "readWrite", db: "test" },
             { role: "read", db: "reporting" } ]
  }
)
```

Connect to the instance and authenticate as `myTester`.[¶](https://docs.mongodb.com/manual/tutorial/enable-authentication/#connect-to-the-instance-and-authenticate-as-mytester)

```
mongo --port 27017 -u "myTester" --authenticationDatabase "test" -p
```

or

```
mongo --port 27017
use test
db.auth("myTester", passwordPrompt())  // or cleartext password
```

## [Role-Based Access Control](https://docs.mongodb.com/manual/core/authorization/)

### [Manage Users and Roles](https://docs.mongodb.com/manual/tutorial/manage-users-and-roles/)

#### **Create a Role to Manage Current Operations[¶](https://docs.mongodb.com/manual/tutorial/manage-users-and-roles/#create-a-role-to-manage-current-operations)**

Connect to MongoDB with the appropriate privileges.[¶](https://docs.mongodb.com/manual/tutorial/manage-users-and-roles/#connect-to-mongodb-with-the-appropriate-privileges)

```
mongo --port 27017 -u myUserAdmin -p 'abc123' --authenticationDatabase 'admin'
```

Create a new role to manage current operations.[¶](https://docs.mongodb.com/manual/tutorial/manage-users-and-roles/#create-a-new-role-to-manage-current-operations)

```
use admin
db.createRole(
   {
     role: "manageOpRole",
     privileges: [
       { resource: { cluster: true }, actions: [ "killop", "inprog" ] },
       { resource: { db: "", collection: "" }, actions: [ "killCursors" ] }
     ],
     roles: []
   }
)
```

#### **Create a Role to Run `mongostat`[¶](https://docs.mongodb.com/manual/tutorial/manage-users-and-roles/#create-a-role-to-run-mongostat)**

Connect to MongoDB with the appropriate privileges.[¶](https://docs.mongodb.com/manual/tutorial/manage-users-and-roles/#id4)

```
mongo --port 27017 -u myUserAdmin -p 'abc123' --authenticationDatabase 'admin'
```

Create a new role to manage current operations.[¶](https://docs.mongodb.com/manual/tutorial/manage-users-and-roles/#id6)

```
use admin
db.createRole(
   {
     role: "mongostatRole",
     privileges: [
       { resource: { cluster: true }, actions: [ "serverStatus" ] }
     ],
     roles: []
   }
)
```

**Create a Role to Drop `system.views` Collection across Databases[¶](https://docs.mongodb.com/manual/tutorial/manage-users-and-roles/#create-a-role-to-drop-system-views-collection-across-databases)**

Connect to MongoDB with the appropriate privileges.[¶](https://docs.mongodb.com/manual/tutorial/manage-users-and-roles/#id8)

```
mongo --port 27017 -u myUserAdmin -p 'abc123' --authenticationDatabase 'admin'
```

Create a new role to drop the `system.views` collection in any database.[¶](https://docs.mongodb.com/manual/tutorial/manage-users-and-roles/#create-a-new-role-to-drop-the-system-views-collection-in-any-database)

```
use admin
db.createRole(
   {
     role: "dropSystemViewsAnyDatabase",
     privileges: [
       {
         actions: [ "dropCollection" ],
         resource: { db: "", collection: "system.views" }
       }
     ],
     roles: []
   }
)
```

#### Modify Access for an Existing User[¶](https://docs.mongodb.com/manual/tutorial/manage-users-and-roles/#modify-access-for-an-existing-user)

Connect to MongoDB with the appropriate privileges.[¶](https://docs.mongodb.com/manual/tutorial/manage-users-and-roles/#id11)

```
mongo --port 27017 -u myUserAdmin -p 'abc123' --authenticationDatabase 'admin'
```

Identify the user’s roles and privileges.[¶](https://docs.mongodb.com/manual/tutorial/manage-users-and-roles/#identify-the-user-s-roles-and-privileges)

```
use reporting
db.getUser("reportsUser")
```

To display the privileges granted to the user by the `readWrite` role on the `"accounts"` database, issue:

```
use accounts
db.getRole( "readWrite", { showPrivileges: true } )
```

Identify the privileges to grant or revoke.

Modify the user’s access.[¶](https://docs.mongodb.com/manual/tutorial/manage-users-and-roles/#modify-the-user-s-access)

Revoke a Role[¶](https://docs.mongodb.com/manual/tutorial/manage-users-and-roles/#revoke-a-role)

```
use reporting
db.revokeRolesFromUser(
    "reportsUser",
    [
      { role: "readWrite", db: "accounts" }
    ]
)
```

Grant a Role[¶](https://docs.mongodb.com/manual/tutorial/manage-users-and-roles/#grant-a-role)

```
use reporting
db.grantRolesToUser(
    "reportsUser",
    [
      { role: "read", db: "accounts" }
    ]
)
```

#### Modify the Password for an Existing User[¶](https://docs.mongodb.com/manual/tutorial/manage-users-and-roles/#modify-the-password-for-an-existing-user)

Connect to MongoDB with the appropriate privileges.[¶](https://docs.mongodb.com/manual/tutorial/manage-users-and-roles/#id17)

```
mongo --port 27017 -u myUserAdmin -p 'abc123' --authenticationDatabase 'admin'
```

Change the password.[¶](https://docs.mongodb.com/manual/tutorial/manage-users-and-roles/#change-the-password)

```
db.changeUserPassword("reporting", "SOh3TbYhxuLiW8ypJPxmt1oOfL")
```

#### View a User’s Roles[¶](https://docs.mongodb.com/manual/tutorial/manage-users-and-roles/#view-a-user-s-roles)

```
use reporting
db.getUser("reportsUser")
```

#### View a Role’s Privileges[¶](https://docs.mongodb.com/manual/tutorial/manage-users-and-roles/#view-a-role-s-privileges)

```
use products
db.getRole( "read", { showPrivileges: true } )
```

# [Replication](https://docs.mongodb.com/manual/replication/#replication)

## [Replica Set Deployment Tutorials](https://docs.mongodb.com/manual/administration/replica-set-deployment/#replica-set-deployment-tutorials)

### [Deploy a Replica Set](https://docs.mongodb.com/manual/tutorial/deploy-replica-set/)

```
sudo vi /etc/mongod.conf
```

update bindIp

```
# network interfaces
net:
  port: 27017
  bindIp: 0.0.0.0
```

restart service

```
sudo systemctl restart mongod
```

```
rs.initiate( {
   _id : "rs0",
   members: [
      { _id: 0, host: "mongodb0.example.net:27017" },
      { _id: 1, host: "mongodb1.example.net:27017" },
      { _id: 2, host: "mongodb2.example.net:27017" }
   ]
})
rs.conf()
rs.status()
```

On slave

```
rs.slaveOk()
show collections
db.printSlaveReplicationInfo()
rs.add('10.20.20.5:27017')
rs.add({"_id":3,"host":"10.20.20.5:27017","priority":0,"hidden":true}
rs.add({"_id":3,"host":"10.20.20.5:27017","priority":0,"slaveDelay":60})
rs.remove('10.20.20.5:27017')
rs.addArb('192.168.168.131:27017')
rs.add({"_id":3,"host":"10.20.20.5:27017","arbiterOnly":true})
```

On Primary

```
use admin
db.shutdownServer()
```

# [Sharding](https://docs.mongodb.com/manual/sharding/#sharding)

## [Deploy a Sharded Cluster](https://docs.mongodb.com/manual/tutorial/deploy-shard-cluster/#deploy-a-sharded-cluster)

```
sudo mkdir -p /data/mongodb/data/configServer
sudo mkdir -p /data/mongodb/data/shard1
sudo mkdir -p /data/mongodb/data/shard2
sudo mkdir -p /data/mongodb/data/shard3
sudo mkdir -p /data/mongodb/log
sudo mkdir -p /data/mongodb/pid
```

### Create the Config Server Replica Set[¶](https://docs.mongodb.com/manual/tutorial/deploy-shard-cluster/#create-the-config-server-replica-set)

If using a configuration file, set:

```
sharding:
  clusterRole: configsvr
replication:
  replSetName: <replica set name>
net:
  bindIp: localhost,<hostname(s)|ip address(es)>
```

If using the command line options, start the [`mongod`](https://docs.mongodb.com/manual/reference/program/mongod/#bin.mongod) with the `--configsvr`, `--replSet`, `--bind_ip`, and other options as appropriate to your deployment. For example:

```
sudo mongod --configsvr --replSet rscs --dbpath /data/mongodb/data/configServer --bind_ip 0.0.0.0
```

Connect to one of the config servers.[¶](https://docs.mongodb.com/manual/tutorial/deploy-shard-cluster/#connect-to-one-of-the-config-servers)

```
mongo --port 27019
```

Initiate the replica set.[¶](https://docs.mongodb.com/manual/tutorial/deploy-shard-cluster/#initiate-the-replica-set)

```
rs.initiate(
  {
    _id: "rscs",
    configsvr: true,
    members: [
      { _id : 0, host : "server1:27019" },
      { _id : 1, host : "server2:27019" },
      { _id : 2, host : "server3:27019" }
    ]
  }
)
```

### Create the Shard Replica Sets[¶](https://docs.mongodb.com/manual/tutorial/deploy-shard-cluster/#create-the-shard-replica-sets)

Start each member of the shard replica set.[¶](https://docs.mongodb.com/manual/tutorial/deploy-shard-cluster/#start-each-member-of-the-shard-replica-set)

If using a configuration file, set:

```
sharding:
    clusterRole: shardsvr
replication:
    replSetName: <replSetName>
net:
    bindIp: localhost,<ip address>
```

If using the command line option, start the [`mongod`](https://docs.mongodb.com/manual/reference/program/mongod/#bin.mongod) with the `--replSet`, and `--shardsvr`, `--bind_ip` options, and other options as appropriate to your deployment. For example:

```
mongod --shardsvr --replSet <replSetname>  --dbpath <path> --bind_ip localhost,<hostname(s)|ip address(es)>
```

For example:

```
sudo mongod --shardsvr --replSet rsss --dbpath /data/mongodb/data/shard1 --bind_ip 0.0.0.0
sudo mongod --shardsvr --replSet rsss2 --dbpath /data/mongodb/data/shard2 --bind_ip 0.0.0.0  --port 27028
sudo mongod --shardsvr --replSet rsss3 --dbpath /data/mongodb/data/shard3 --bind_ip 0.0.0.0  --port 27038
```

Connect to one member of the shard replica set.[¶](https://docs.mongodb.com/manual/tutorial/deploy-shard-cluster/#connect-to-one-member-of-the-shard-replica-set)

Initiate the replica set.[¶](https://docs.mongodb.com/manual/tutorial/deploy-shard-cluster/#id1)

```
mongo --port 27018
rs.initiate(
  {
    _id: "rsss",
    members: [
      { _id : 0, host : "server1:27018" },
      { _id : 1, host : "server2:27018" },
      { _id : 2, host : "server3:27018" }
    ]
  }
)
mongo --port 27028
rs.initiate(
  {
    _id: "rsss2",
    members: [
      { _id : 0, host : "server1:27028" },
      { _id : 1, host : "server2:27028" },
      { _id : 2, host : "server3:27028" }
    ]
  }
)
mongo --port 27038
rs.initiate(
  {
    _id: "rsss3",
    members: [
      { _id : 0, host : "server1:27038" },
      { _id : 1, host : "server2:27038" },
      { _id : 2, host : "server3:27038" }
    ]
  }
)
```

### Start a mongos for the Sharded Cluster[¶](https://docs.mongodb.com/manual/tutorial/deploy-shard-cluster/#start-a-mongos-for-the-sharded-cluster)

If using a configuration file, set the [`sharding.configDB`](https://docs.mongodb.com/manual/reference/configuration-options/#sharding.configDB) to the config server replica set name and at least one member of the replica set in `<replSetName>/<host:port>` format.

```
sharding:
  configDB: <configReplSetName>/cfg1.example.net:27019,cfg2.example.net:27019
net:
  bindIp: localhost,<hostname(s)|ip address(es)>
```

If using command line parameters start the [`mongos`](https://docs.mongodb.com/manual/reference/program/mongos/#bin.mongos) and specify the `--configdb`, `--bind_ip`, and other options as appropriate to your deployment. For example:

```
mongos --configdb <configReplSetName>/cfg1.example.net:27019,cfg2.example.net:27019,cfg3.example.net:27019 --bind_ip localhost,<hostname(s)|ip address(es)>
```

For example:

```
sudo mongos --configdb rscs/server1:27019,server2:27019,server3:27019 --bind_ip 0.0.0.0 --port 27016
```

Connect to the Sharded Cluster[¶](https://docs.mongodb.com/manual/tutorial/deploy-shard-cluster/#connect-to-the-sharded-cluster)

```
mongo --port 27016
```

Add Shards to the Cluster[¶](https://docs.mongodb.com/manual/tutorial/deploy-shard-cluster/#add-shards-to-the-cluster)

```
sh.addShard( "rsss/server1:27018,server2:27018,server3:27018")
sh.addShard( "rsss2/server1:27028,server2:27028,server3:27028")
sh.addShard( "rsss3/server1:27038,server2:27038,server3:27038")
```

### Enable Sharding for a Database[¶](https://docs.mongodb.com/manual/tutorial/deploy-shard-cluster/#enable-sharding-for-a-database)

```
sh.enableSharding("test")
```

### Shard a Collection[¶](https://docs.mongodb.com/manual/tutorial/deploy-shard-cluster/#shard-a-collection)

```
sh.shardCollection("test.testshard", { item : "hashed" } )
```

```
db.testshard.insertMany([
   { item: "journal", qty: 25, tags: ["blank", "red"], dim_cm: [ 14, 21 ] },
   { item: "notebook", qty: 50, tags: ["red", "blank"], dim_cm: [ 14, 21 ] },
   { item: "paper", qty: 100, tags: ["red", "blank", "plain"], dim_cm: [ 14, 21 ] },
   { item: "planner", qty: 75, tags: ["blank", "red"], dim_cm: [ 22.85, 30 ] },
   { item: "postcard", qty: 45, tags: ["blue"], dim_cm: [ 10, 15.25 ] }
]);
db.testshard.stats()
```

# [Administration](https://docs.mongodb.com/manual/administration/#administration)

## [MongoDB Backup Methods](https://docs.mongodb.com/manual/core/backups/#mongodb-backup-methods)

### [Back Up and Restore with Filesystem Snapshots](https://docs.mongodb.com/manual/tutorial/backup-with-filesystem-snapshots/#back-up-and-restore-with-filesystem-snapshots)

### [Back Up and Restore with MongoDB Tools](https://docs.mongodb.com/manual/tutorial/backup-and-restore-tools/#back-up-and-restore-with-mongodb-tools)

[`mongodump`](https://docs.mongodb.com/manual/reference/program/mongodump/#bin.mongodump) only captures the documents in the database. The resulting backup is space efficient, but [`mongorestore`](https://docs.mongodb.com/manual/reference/program/mongorestore/#bin.mongorestore) or [`mongod`](https://docs.mongodb.com/manual/reference/program/mongod/#bin.mongod) must rebuild the indexes after restoring data.

When you run [`mongodump`](https://docs.mongodb.com/manual/reference/program/mongodump/#bin.mongodump) without any arguments, the command connects to the MongoDB instance on the local system (e.g. `localhost`) on port `27017` and creates a database backup named `dump/` in the current directory.

```
mongodump
mongodump --oplog --out=/data/backup/
mongodump --host=mongodb.example.net --port=27017
mongodump --out=/data/backup/
mongodump --collection=myCollection --db=test
mongodump --host=mongodb1.example.net --port=3017 --username=user --password="pass" --out=/opt/backup/mongodump-2013-10-24
mongorestore --port=<port number> <path to the backup>
mongorestore dump-2013-10-25/
mongorestore --oplogReplay
mongorestore --host=mongodb1.example.net --port=3017
mongorestore --host=mongodb1.example.net --port=3017 --username=user  --authenticationDatabase=admin /opt/backup/mongodump-2013-10-24
```

# [Reference](https://docs.mongodb.com/manual/reference/)

## [mongo Shell Methods](https://docs.mongodb.com/manual/reference/method/)

### [User Management](https://docs.mongodb.com/manual/reference/method/#user-management)

#### [db.getUser()](https://docs.mongodb.com/manual/reference/method/db.getUser/#db.getUser)

```
use accounts
db.getUser("appClient")
```

#### [db.getUsers()](https://docs.mongodb.com/manual/reference/method/db.getUsers/#db.getUsers)

```
db.getUsers()
db.getUsers({ filter: { mechanisms: "SCRAM-SHA-256" } })
```

# [Ops Manager](https://docs.opsmanager.mongodb.com/current/)