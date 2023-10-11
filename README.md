###@@@@@Dockerized WordPress Assignment@@@@@

This repository contains the Dockerfile, Docker Compose file, .env file and database optimization strategies for Dockerizing a WordPress application. The assignment's objective was to create a Dockerfile for WordPress, write a Docker Compose file to orchestrate the application, optimize the database for performance, and document the approach in a Readme file.


creation of Dockerfile for WordPress

I have created a Dockerfile for WordPress,In this Dockerfile, I use the official WordPress image as the base image, set a maintainer label, and expose port 8083.

##############################################

FROM wordpress:latest

LABEL maintainer="manjunath"

EXPOSE 8083

##############################################

creation of .env file

I have created a .env file in the same directory as my Docker Compose file and defined the environment variables in the .env file


##############################################

MYSQL_ROOT_PASSWORD=manju65char
MYSQL_DATABASE=wordpress
MYSQL_USER=wordpress
MYSQL_PASSWORD=wordpress

##############################################

Database Optimization for Performance

Database optimization is crucial for enhancing performance and implemented the following strategies:

Indexing:  added indexes to the database tables to accelerate query performance.

CREATE INDEX idx_post_title ON wp_posts (post_title);


Caching: the caching settings are configured to reduce the load on the database server and improve response times.

-- Set query cache size (e.g., 64MB)
SET GLOBAL query_cache_size = 67108864;

-- Enable query caching
SET GLOBAL query_cache_type = 1;

Query Optimization: Database queries have been optimized to minimize resource consumption and response time.

OPTIMIZE TABLE wp_posts;



##############################################


creation of Docker Compose File

I have created Docker Compose file (docker-compose.yml) that orchestrates the WordPress application and the database (MySQL):


##############################################

version: '3'

services:
  db:
    image: mysql:5.7
    volumes:
      - ./mysql-data:/var/lib/mysql
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: ${MYSQL_ROOT_PASSWORD}
      MYSQL_DATABASE: ${MYSQL_DATABASE}
      MYSQL_USER: ${MYSQL_USER}
      MYSQL_PASSWORD: ${MYSQL_PASSWORD}
    networks:
      - wpdbnetwork

  wordpress:
    depends_on:
      - db
    build:
      context: . 
      dockerfile: Dockerfile  
    restart: always
    environment:
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_USER: ${MYSQL_USER}
      WORDPRESS_DB_PASSWORD: ${MYSQL_PASSWORD}
      WORDPRESS_DB_NAME: ${MYSQL_DATABASE}
    volumes:
      - ./wp-content:/var/www/html/wp-content
    networks:
      - wpdbnetwork
    ports:
      - "8083:80"  

networks:
  wpdbnetwork:

volumes:
  wpdbnetwork:


##############################################

Now, the Docker Compose file references the environment variables defined in the .env file. When you run docker-compose, it will automatically pick up the values from the .env file for the environment variables

##############################################

How to Build and Run the application 


To build and run the Dockerized WordPress application, follow these steps:

1.Clone this repository.

2.Ensure you have Docker and Docker Compose installed.

3.Navigate to the repository's root directory.


cd <path of the directory> 

4.Run the following command to build and start the application

docker-compose up -d

5.Access the WordPress application in your browser at http://localhost:8083 or http://pubicip:8083






