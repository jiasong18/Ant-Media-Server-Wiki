## If you use a Local database (internal database), please follow the following instructions.

#### Step.1: Go to the installation directory of Ant Media Server. 

`
cd /usr/local/antmedia
`

#### Step.2: Remove "server.db" file. 

`
sudo rm server.db
`

#### Step.3: Restart Ant Media Server.

`
sudo service antmedia restart
`



## If you use MongoDB, please follow the following instructions.

#### Step.1: Connect to the MongoDB server

`
mongo
`

#### Step.2: Go to the serverdb database

`
use serverdb;
`

#### Step.3: Find the user

`db.User.find()`

Output:

`{ "_id" : ObjectId("5ea486690f09e71c2462385a"), "className" : "io.antmedia.rest.model.User", "email" : "test@antmedia.io", "password" : "1234567", "userType" : "ADMIN" }`

#### Step.4: You can delete or update the row

We updated our password as test123

`db.User.updateOne( { email:"test@antmedia.io" }, { $set: { "password" : "test123" }})`

If you delete the user, you will create a new user next time you login on the dasboard.

`db.User.deleteOne(     { "email": "test@antmedia.io" }  )`

If you are still having issues, please let us know that. 
[contact@antmedia.io](mailto:contact@antmedia.io) 
