## Overview: What Are We Doing?

### *We are setting up a Python application that interacts with a MySQL database. The task involves:*

1. Creating a custom Docker network for communication between containers.
2. Running a MySQL container that hosts the database.
3. Building a Docker image for a Python script (test.py) that creates a database, a 
   table, and inserts records.
4. Running the Python container and connecting it to the MySQL container via the 
    network.
5. Verifying that the database and table are successfully created.

*This process demonstrates how to link multiple containers and manage application interactions using Docker.*


## Follow below steps to Implement.

### step 1. Create test.py
*This Python script:*

- Connects to the MySQL container using the mysql-connector-python library.
- Creates a database (test_db), a table (users), and inserts a sample record.
- Verifies the setup by fetching and printing data.


*Code:*

```yml
import mysql.connector
import time

# Wait to ensure MySQL is ready
time.sleep(10)

db_config = {
    'host': 'mysql-container',  # MySQL container name
    'user': 'root',
    'password': 'rootpassword'
}

try:
    # Connect to MySQL and perform operations
    connection = mysql.connector.connect(**db_config)
    cursor = connection.cursor()

    cursor.execute("CREATE DATABASE IF NOT EXISTS test_db")
    cursor.execute("USE test_db")
    cursor.execute("""
        CREATE TABLE IF NOT EXISTS users (
            id INT AUTO_INCREMENT PRIMARY KEY,
            name VARCHAR(100),
            email VARCHAR(100)
        )
    """)
    cursor.execute("INSERT INTO users (name, email) VALUES (%s, %s)", ("John Doe", "john.doe@example.com"))
    connection.commit()

    cursor.execute("SELECT * FROM users")
    for row in cursor.fetchall():
        print(f"User: {row}")
except mysql.connector.Error as err:
    print(f"Error: {err}")
finally:
    if connection.is_connected():
        cursor.close()
        connection.close()


```




### step 2. Create a Dockerfile
*This file packages test.py into a Docker image.*

  - It uses a lightweight Python base image.
  - Copies test.py into the container.
  - Installs the required library (mysql-connector-python).
  - Specifies that test.py will run when the container starts.
*

Dockerfile:*


```yml

FROM python:3.9-slim
WORKDIR /usr/src/app
COPY test.py .
RUN pip install mysql-connector-python
CMD ["python", "test.py"]

```


### step 3. Create a Docker Network
  *To allow the Python and MySQL containers to communicate, we create a custom Docker network.*

```yml
docker network create app2
```

### step 4. Run the MySQL Container
  *Start a MySQL container with:*

  - A root password (rootpassword).
  - The app2 network connection.
    
```yml
docker run -d --name mysql-container --network app2 -e MYSQL_ROOT_PASSWORD=rootpassword mysql:8

```

### step 5. Build the Docker Image for test.py
   *Build the image for your Python application.*

```yml
docker build -t test-python-app .
```

### step 6. Run the Python Container
   *Run the container for test.py, connecting it to the app2 network so it can access the MySQL container.*

```yml
docker run -d --name test-python-container --network app2 test-python-app
```

### step 7. Verify the Setup
  *Access the MySQL container to confirm the database and table were created.*

1: Access the MySQL container:

```yml
docker exec -it mysql-container bash
```

2: Log in to MySQL:

```yml
mysql -u root -p
```
  *Enter the password: rootpassword.*


3: Check the database and table:

```yml
USE test_db;
SHOW TABLES;
SELECT * FROM users;
```


<br>
<br>

### What Did We Learn?
1. How to Dockerize Applications:

  - We created a Docker image for a Python script and linked it with a database.

2. Networking in Docker:

  - We learned to create a custom Docker network (app2) to enable container     
    communication.

3. Database Integration with Docker:

  - The Python application successfully created and interacted with a MySQL database     running in a separate container.

4. Hands-On with Docker Commands:
  - We used commands to create networks, run containers, and verify their   
   interactions.
    
By the end, you have a Python application running in a container that communicates with a MySQL database container seamlessly.



<br>
<br>
<br>



## ------------------Screnshots--------------------
1.
<br>
<br>


![Alt text for image](screenshots/1.png)

2.
<br>
<br>


![Alt text for image](screenshots/2.png)


3.
<br>
<br>


![Alt text for image](screenshots/3.png)

<br>
<br>

























