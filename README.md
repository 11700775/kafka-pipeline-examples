# kafka-pipeline-examples
**Prerequisites:**
* This project uses Docker and docker-compose. View [this link](https://docs.docker.com/compose/install/) to find out how to install them for your OS.

* Before we begin, create a new environment. I use Anaconda to do this but feel free to use any tool of your liking.     Activate the environment and install the required libraries by executing the following command:
        
        pip install -r requirements.txt

## [Producing and consuming data to Kafka](https://github.com/Wesley-Bos/kafka-pipeline-examples/tree/master/producer-consumer)
1. Run the docker-compose.yml file:
        
        docker-compose -f docker-compose.yml up -d
2. Wait a moment while Docker downloads the images and for the services to start up.
3. Create the **producer**:
        
        python producer.py
4. Create the **consumer**:

        python consumer.py
   In the terminal, every message that the producer has published is displayed.
   
   Data is now being **produced** and **consumed** from Kafka.
   
5. By going to the [Control Center](http://localhost:9021), the same data can be seen in a UI.
   
   *"Confluent Control Center is a web-based tool for managing and monitoring Apache Kafka®. Control Center facilitates building and monitoring production data pipelines and streaming applications."*

## Source and Sink with Kafka using PostgreSQL
1. Run the docker-compose.yml file:

        docker-compose -f docker-compose.yml up -d
2. Setup the Postgres database:
        
        docker-compose -f docker-compose-pg.yml up -d
3. Write data to the database:

        python generate_data.py
        
   View the data in the database by executing the following commands:    
   * Connect to database:
         
         docker exec -it postgres psql -U pxl numbers
   * View the databases:       
   
         numbers=# \dt
   * View the tables:
         
         numbers=# \l
   * View the data:
   
         numbers=# 
4. Create a **source** connector:
  
       ./connect.sh
   When the connector is successfully created, the message "HTTP/1.1 201 Created" will be shown in the terminal.
   The status of the connectors can also be viewed in the [Control Center](http://localhost:9021).
5. Create a **sink** connector:
  
        ./delete_sink.sh
   When the connector is successfully created, the message "HTTP/1.1 201 Created" will be shown in the terminal.
6. Go back to the Postgres database and view all tables for the database numbers. The table 'P_COUNTER' should be added. 
    
    To see if the data is streaming, execute the queries below, do this alongside each other.
   
        SELECT COUNT(index) FROM "COUNTER";
        SELECT COUNT(index) FROM "P_COUNTER";
   
    The data is now streaming from the table COUNTER to the table P_COUNTER.
