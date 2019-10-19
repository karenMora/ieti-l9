# ieti-l9


#----------------------------------------------------------------------------
# jpa-mongodb
Create a Spring Boot Application that connects with MongoDB.


## Part 1: Basic Mongo DB configuration and Spring Boot Integration
1. Create a free MongoDB Atlas account on:

    https://www.mongodb.com/atlas-signup-from-mlab
    
2. Configure the MongoDB Cluster:

    * Create new Organization
    
        ![](img/step-1-a.png)
        
        ![](img/step-1-b.png)
    
    * Create a new Starter Cluster (free) using any Cloud Provider and Region
   
        ![](img/step-2.png)

    * Go to Database Access menu on the left panel and create a user and a password
    
        ![](img/step-3.png)
        
    * Go to Network Access menu on the left panel and add your IP so it lets the application connect from your current IP address
    
        ![](img/step-4.png)
        
    * Go to the cluster menu on the left panel and click on the Connect button
    
        ![](img/step-5.png)
        
    * Select the option Connect your Application and then copy the connection String only(make sure you replace the \<password\> tag with the created user password)
        
        ![](img/step-5-a.png)
        
        ![](img/step-5-b.png)
        
        
    
        
3. Download the sample project and replace the spring.data.mongodb.uri value on application.properties file with the connection String copied on step 2)


4. Run the project and verify that the connection is correct.


5. Create two more models (User and Todo) with the following structure:

    User
    ````Javascript
        
        {
            "id": "12354",
            "name": "Charles Darwin",
            "email": "charles@natural.com"
        }
        
     
    ````     
    
    Todo
    ````Javascript
        
        {
            "description": "travel to Galapagos",
            "priority": 10,
            "dueDate": "Jan 10 - 1860"
            "responsible": "charles@natural.com"
            "status": "pending"
        }
    ````                  
    
    
6. Create a repository for each model(use the *CustomerRepository* as reference)

7. Add a method to the TodoRepository *findByResponsible* and verify it works (write a unit test for this method)

## Part 2: Custom configuration and Queries

1. Create a configuration class with the following code:

    ````java


    @Configuration
    public class AppConfiguration {
    
        @Bean
        public MongoDbFactory mongoDbFactory() throws Exception {
    
             MongoClientURI uri = new MongoClientURI(
            "mongodb+srv://sancarbar:<password>@cluster0-dzkk5.mongodb.net/test?retryWrites=true&w=majority");

            MongoClient mongoClient = new MongoClient(uri);

            return new SimpleMongoDbFactory( mongoClient, "test");
    
        }
    
        @Bean
        public MongoTemplate mongoTemplate() throws Exception {
    
            MongoTemplate mongoTemplate = new MongoTemplate(mongoDbFactory());
    
            return mongoTemplate;
    
        }
    
    }
    
    ````

2. Replace the credential values and the server address.

3. Add the following code to your Application run method to access the *MongoTemplate* object:

    ````java
    
        ApplicationContext applicationContext = new AnnotationConfigApplicationContext(AppConfiguration.class);
        MongoOperations mongoOperation = (MongoOperations) applicationContext.getBean("mongoTemplate");
     
    ````     
    
4. The *MongoOperations* instance allows you to create custom queries to access the data by using the *Query* object:
 
    ````java
    
       Query query = new Query();
       query.addCriteria(Criteria.where("firstName").is("Alice"));
    
       Customer customer = mongoOperation.findOne(query, Customer.class);
     
    ````  

5. Read some of the documentation about queries in Spring Data MongoDB:
 
    * https://www.baeldung.com/queries-in-spring-data-mongodb
    * https://www.mkyong.com/mongodb/spring-data-mongodb-query-document/

6. Create mocked data for 25 Todos and 10 different users (make sure the Todos have different dueDates and responsible)

7. Create the following queries using the Query class:

    * Todos that the dueDate has expire
    * Todos that are assigned to given user and have priority greater equal to 5
    * List users that have assigned more than 2 Todos.
    * Todo list that contains the description with a length greater than 30 characters           
    
    

