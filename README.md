 
# Flask App with MySQL Docker Setup

This is a simple Flask app that interacts with a MySQL database. The app allows users to submit messages, which are then stored in the database and displayed on the frontend.

## Prerequisites

Before you begin, make sure you have the following installed:

- Docker
- Git (optional, for cloning the repository)

## Setup

1. Clone this repository (if you haven't already):

  git clone https://github.com/devopsplan2026/two-tier-flask-docker-project.git


2. Navigate to the project directory:

   cd your-repo-name

   - First create a docker image from Dockerfile

#### To run this two-tier application using  without docker-compose

3. Create a Image form dockerfile 

docker build -t two .

4. Check and Create a Network 

Run to check docker network list:

docker network ls

docker network create mynet -d bridge    ## -d means driver, these networks we called also driver

5. Run the container mysql

docker run -d --name my_sql --network two -e MYSQL_ROOT_PASSWORD=root -e MYSQL_DATABASE=devops mysql

6. Run the container Flask

docker run -d -p 5000:5000 --network two -e MYSQL_HOST=my_sql -e MYSQL_USER=root -e MYSQL_PASSWORD=root -e MYSQL_DB=devops two:latest

###### To check the logs of container

docker logs <container-ID>

###### To check the logs of docker network 

docker network inspect two

7. Database testing: ( Go inside the container and access the database )

Run: docker exec -it my_sql /bin/bash 

Run: mysql -u root -p 

Run: SHOW DATABASES;

Or 

CREATE DATABASE devops;

Run: USE devops;
Run: SHOW TABLES;
Run: select * from messages;

or 

i) MySQL container 

```bash
docker run -d \
    --name mysql \
    --network=two \
    -e MYSQL_DATABASE=mydb \
    -e MYSQL_ROOT_PASSWORD=admin \
    -p 3306:3306 \
    mysql

    or 

ii) Backend container

```bash
docker run -d \
    --name flaskapp \
    --network=twotier \
    -e MYSQL_HOST=mysql \
    -e MYSQL_USER=root \
    -e MYSQL_PASSWORD=admin \
    -e MYSQL_DB=mydb \
    -p 5000:5000 \
    two:latest

#### To run this two-tier application using  with docker-compose

1. Create a `.env` file in the project directory to store your MySQL environment variables:

   ```bash
   touch .env
   ```

2. Open the `.env` file and add your MySQL configuration:

   ```
   MYSQL_HOST=mysql
   MYSQL_USER=your_username
   MYSQL_PASSWORD=your_password
   MYSQL_DB=your_database
   ```

## Usage

0. Create docker-compose.file




1. Start the containers using Docker Compose:

   ```bash
   docker-compose up --build
   ```

2. Access the Flask app in your web browser:

   - Frontend: http://localhost
   - Backend: http://localhost:5000

3. Create the `messages` table in your MySQL database:

   - Use a MySQL client or tool (e.g., phpMyAdmin) to execute the following SQL commands:
   
     ```sql
     CREATE TABLE messages (
         id INT AUTO_INCREMENT PRIMARY KEY,
         message TEXT
     );
     ```

4. Interact with the app:

   - Visit http://localhost to see the frontend. You can submit new messages using the form.
   - Visit http://localhost:5000/insert_sql to insert a message directly into the `messages` table via an SQL query.

## Cleaning Up

To stop and remove the Docker containers, press `Ctrl+C` in the terminal where the containers are running, or use the following command:

```bash
docker-compose down
```

## Notes

- Make sure to replace placeholders (e.g., `your_username`, `your_password`, `your_database`) with your actual MySQL configuration.
- This is a basic setup for demonstration purposes. In a production environment, you should follow best practices for security and performance.
- Be cautious when executing SQL queries directly. Validate and sanitize user inputs to prevent vulnerabilities like SQL injection.
- If you encounter issues, check Docker logs and error messages for troubleshooting.




 




