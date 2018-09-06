# MongoDB

### Prerequisites
 - Start the mongoDB server using the command `mongod` in the terminal
 - Start the mongoDB interactive shell using the command `mongo` on a separate terminal.

 - > **NOTE** : Sometimes the mongod server is already running in the backgroud. So first just try running the command in the second step. If it doesn't work then start the server then start the interactive shell

After sucessfully entering the interactive shell try out the following stuff :

---

## <ins>CREATE</ins>

#### Creating a database
 - To create a database enter the command `use db_name` where _db_name_ is the name of the database you want to create.
 - This command is also used to switch between databases
 - Eg:-
    ```js
    use cars
    ```

#### List databases
 - To view the various databases created enter :-
    ```js
    show dbs
    ```
 - Result:
    ```js
    admin   0.000GB
    config  0.000GB
    local   0.000GB
    temp    0.000GB
    ```
> **NOTE** : The _cars_ db created in the earlier command isn't visible as it has no data in it. Once a [collection](#creating-collections) is created it will be visible on executing the above command.

#### Creating collections
 - Collections are in `mongoDB` what tables are in `SQL`.
 - The only difference is that you don't need to have a defined structure of the _column names_ and the _datatype_ those columns will have in `mongoDB` unlike `SQL`.
 - You can directly create a collection without specifying any details of the data that will be entered in it.
 - This can be done using the command `db.createCollection('collection_name')`
 - Eg:-
    ```js
    db.createCollection('car_details')
    ```


#### Inserting documents
 - In `mongoDB` when you make an entry in a collection that entry is called a _document_ i.e. a _row_ in terms of `SQL`
 - You can insert a document in a collection using the command `db.collection_name.insert(data)`
 - Structure of the data in the example below :-
   - brand
   - model_name
   - type
   - engine
     - fuel
     - cc
     - bhp
   - color
   - safety
     - airbag (if airbags isn't present then this field is omitted else its type is associated with the _key_)
     - sb_warn (seatbelt warning)
 - Eg:-
    ```js
    db.car_details.insert(
        {
            'brand': 'chevrolet',
                'model_name': 'beat',
            'type': 'hatchback',
            'engine': {
                'fuel': 'petrol',
                'cc': 1000,
                'bhp': 100
            },
            'color': ['red', 'blue', 'green'],
            'safety': {
                'sb_warn': false
            },
            'price' : 400000
        }
    )
    ```
 - You can insert multiple entries in one go by using an array of objects i.e.
    ```js
    db.collection_name.insert([{},{},....])
    ```
 - Copy and paste the contents of [this](insert.js) file in your terminal.
   - The file contains an array of objects containing the car_details.

---

## <ins>READ</ins>

 - Fetching data from the documents is done using the `find()` function.
 - Eg:- 
    ```
    db.car_details.find()
    ```
 - This returns all the documents in the _car_details_ collection as **no** parameter was passed as to which filter(s) should be applied.

- To get a readable format of the results use the `pretty()` function
- Eg: -
    ```js
    db.car_details.find().pretty()
    ```
- This returns a properly formatted result of the `find()` output.

#### Filters

 - To get the document(s) based on some filters enter the _key_ and the corresponding _value_ as the first parameter to the `find()` function.
 - Eg:- 
 - Here we want to get the details of the cars of type **sedan**
    ```js
    db.car_details.find({ 'type' : 'sedan' }).pretty()
    ```
 - You could relate this type of filtering to the _WHERE_ clause in `SQL`.

#### Comparison Operators

  - Comparison operators can be used to filter documents in monogDB using the following specifiers:-
    -  **$gt** = greater than
    -  **$gte** = greater than equal to
    -  **$lt** = less than
    -  **$lte** = less than equal to
    -  **$ne** = not equal to
  - Eg:- (**$lte**)
    ```js
    db.car_details.find({
        'price' : { '$lte' : 500000 }
    }).pretty()
    ```

#### Logical Operators

 - Logical operators can be used to filter documents in monogDB using the following specifications:-
   - **$or** = OR operation
   - **$and*** = AND operation

 - Eg:- (AND)
    ```js
    db.car_details.find({'$and' : [
        { 'type' : 'sedan' },
        { 'price' : { '$gte' : 500000 } }
    ]}).pretty()
    ```
    > Notice that the following command wil also output the same as the previous as the default operation in case of multiple filters is an _AND_ operation.
 - Eg:- (AND)(without the **$and** specifier)
    ```js
    db.car_details.find({
         'type' : 'sedan' ,
         'price' : { '$gte' : 500000 }
    }).pretty()
    ```
 - Eg:- (OR)
    ```js
    db.car_details.find({'$or': [
        { 'type' : 'sedan' },
        { 'price' : { '$gte' : 500000 } }
    ]}).pretty()
    ```

**sort()**
 - Sort funtion can be used to sort the result of the query based on a key
 - For ascending set the value of the key to 1
   and for descending set it to -1
 - Eg:- SORT (desc)
    ```js
    db.car_details.find().sort(
        {'price' : -1}
    ).pretty()
    ```

**limit()**
 - To limit the no of results of the query send the no of results to be displayed as a parameter to the limit function
 - Eg:-
    ```js
    db.car_details.find().limit(3).pretty()
    ```

**count()**
 - To just display the no of results that the query will output the `count()` function can be used
 - Eg:-
    ```js
    db.car_details.find().count()
    ```