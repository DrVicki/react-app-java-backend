# react-app-java-backend
How To Develop and Build React App With Java Backend

There are many ways to build React apps and ship for production. 
  - One way is to build React with NodeJS or Java, and 
  - another way is to build the React and serve that static content with NGINX web server. 
  - With Java we have to deal with the server code as well; you need to load ```index.html``` page with java.

In this tutorial project, we will see details and implementation with Java. We will go through step by step with an example.
  - Introduction
  - Prerequisites
  - Example Project
  - How To Build and Develop The Project
  - How To Build For Production
  - Summary
  - Conclusion


## Introduction

React is a javascript library for building web apps and it doesn’t load itself in the browser. We need a mechanism which loads the ```index.html``` (single page) of React with all the dependencies (CSS and js files) in the browser. In this case, we are using java as the webserver which loads React assets and accepts any API calls from the React app.

![](https://github.com/DrVicki/react-app-java-backend/blob/main/images/react-with-java.png)

If you look at the above diagram all the web requests without the ```/api``` will go to React router. All the paths that contain ```/api``` will be handled by the Apache Tomcat container.

## Prerequisites

There are prerequisites for this tutorial. You need to have java installed on your laptop. If you want to practice and run this on your laptop you need to have these on your laptop.

  - Java
  - Create React App
  - VSCode
  - Eclipse IDE
  - react-bootstrap
  - Maven

# Example Project

This is a simple project which demonstrates developing and running a React application with Java. We have a simple app. We can add users, count, display them at the side, and retrieve them whenever you want.

![](https://github.com/DrVicki/react-app-java-backend/blob/main/images/example-project.gif)

As you add users we are making an API call to the Java server to store them, and get the same data from the server when we retrieve them. You can see network calls in the following video.

![](https://github.com/DrVicki/react-app-java-backend/blob/main/images/network-calls.gif)

## How To Build and Develop The Project

Usually, the way you develop and the way you build and run in production are completely different. I would like to define two phases: Development phase and Production phase.

  - In the development phase, we run the java server and the React app on completely different ports. It’s easier and faster to develop that way. If you look at the following diagram the React app is running on port 3000 with the help of a webpack dev server and the java server is running on port 8080.
  - ![](https://github.com/DrVicki/react-app-java-backend/blob/main/images/dev-env.png)

## Project Structure

Let’s look at the project structure for this project. We need to have two completely different folders for java and react. It’s always best practice to have completely different folders for each one. In this way, you will have a clean architecture.

![](https://github.com/DrVicki/react-app-java-backend/blob/main/images/proj-struct.png)


![](https://github.com/DrVicki/react-app-java-backend/blob/main/images/proj-stru-2.png)

If you look at the above project structure, all the React app resides under the ```src/main/ui``` folder and Java code resides under the ```src/main/java``` folder. All the resources are under the folder ```/src/main/resources``` such as properties, static assets, etc.

## Java API

We use spring boot and many other tools such as Spring Devtools, Spring Actuator, etc under the spring umbrella. Almost every application has spring boot and it is an open-source Java-based framework used to create a micro Service. It is developed by the Pivotal Team and is used to build stand-alone and production-ready spring applications.

We start with Spring initializr and select all the dependencies and generate the zip file.

![](https://github.com/DrVicki/react-app-java-backend/blob/main/images/spring-init.png)

Once you import the zip file in eclipse or any other IDE as a Maven project you can see all the dependencies in the ```pom.xml```. Below is the dependencies section of ```pom.xml```.

**pom.xml**
```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>2.2.6.RELEASE</version>
		<relativePath/> <!-- lookup parent from repository -->
	</parent>
	<groupId>com.bbtutorials</groupId>
	<artifactId>users</artifactId>
	<version>0.0.2-SNAPSHOT</version>
	<name>users</name>
	<description>Demo project for Spring Boot</description>

	<properties>
		<java.version>1.11</java.version>
	</properties>

	<dependencies>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-actuator</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-data-jpa</artifactId>
		</dependency>
		<dependency>
			<groupId>com.h2database</groupId>
			<artifactId>h2</artifactId>
			<scope>runtime</scope>
			<version>1.4.199</version>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-data-rest</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-hateoas</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.data</groupId>
			<artifactId>spring-data-rest-hal-browser</artifactId>
		</dependency>
		<!-- QueryDSL -->
		<dependency>
			<groupId>com.querydsl</groupId>
			<artifactId>querydsl-apt</artifactId>
		</dependency>
		<dependency>
			<groupId>com.querydsl</groupId>
			<artifactId>querydsl-jpa</artifactId>
		</dependency>

		<dependency>
			<groupId>com.h2database</groupId>
			<artifactId>h2</artifactId>
			<scope>runtime</scope>
		</dependency>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-devtools</artifactId>
			<scope>runtime</scope>
			<optional>true</optional>
		</dependency>
		<dependency>
			<groupId>org.projectlombok</groupId>
			<artifactId>lombok</artifactId>
			<optional>true</optional>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
			<exclusions>
				<exclusion>
					<groupId>org.junit.vintage</groupId>
					<artifactId>junit-vintage-engine</artifactId>
				</exclusion>
			</exclusions>
		</dependency>
	</dependencies>

	<build>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
			</plugin>
			<plugin>
				<groupId>com.mysema.maven</groupId>
				<artifactId>apt-maven-plugin</artifactId>
				<version>1.1.3</version>
				<executions>
					<execution>
						<goals>
							<goal>process</goal>
						</goals>
						<configuration>
							<outputDirectory>target/generated-sources/java</outputDirectory>
							<processor>com.querydsl.apt.jpa.JPAAnnotationProcessor</processor>
						</configuration>
					</execution>
				</executions>
			</plugin>
		</plugins>
	</build>

</project>
```

Here are the spring boot file and the controller with two routes one with a GET request and another is POST request.

**UsersApplication.java**
```
package com.bbtutorials.users;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class UsersApplication {

	public static void main(String[] args) {
		SpringApplication.run(UsersApplication.class, args);
	}

}
```

**UsersController.java**
```
package com.bbtutorials.users.controller;

import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

import com.bbtutorials.users.entity.Users;
import com.bbtutorials.users.links.UserLinks;
import com.bbtutorials.users.service.UsersService;

import lombok.extern.slf4j.Slf4j;

@Slf4j
@RestController
@RequestMapping("/api/")
public class UsersController {
	
	@Autowired
	UsersService usersService;
	
	@GetMapping(path = UserLinks.LIST_USERS)
    public ResponseEntity<?> listUsers() {
        log.info("UsersController:  list users");
        List<Users> resource = usersService.getUsers();
        return ResponseEntity.ok(resource);
    }
	
	@PostMapping(path = UserLinks.ADD_USER)
	public ResponseEntity<?> saveUser(@RequestBody Users user) {
        log.info("UsersController:  list users");
        Users resource = usersService.saveUser(user);
        return ResponseEntity.ok(resource);
    }
}
```

## Configure H2 Database

This H2 Database is for development only. When you build this project for production you can replace it with any database of your choice. You can run this database standalone without your application. We will see how we can configure it with spring boot.

First, we need to add some properties to application.properties file under ```/src/main/resources```

```
spring.datasource.url=jdbc:h2:mem:testdb
spring.datasource.driverClassName=org.h2.Driver
spring.datasource.username=sa
spring.datasource.password=password
spring.jpa.database-platform=org.hibernate.dialect.H2Dialect
spring.h2.console.enabled=true
```

Second, add the below SQL file under the same location.

**data.sql** 
```
DROP TABLE IF EXISTS users;

CREATE TABLE users (
  id INT PRIMARY KEY,
  FIRST_NAME VARCHAR(250) NOT NULL,
  LAST_NAME VARCHAR(250) NOT NULL,
  EMAIL VARCHAR(250) NOT NULL
);

INSERT INTO users (ID, FIRST_NAME, LAST_NAME, EMAIL) VALUES
  (1, 'first', 'last 1', 'abc1@gmail.com'),
  (2, 'first', 'last 2', 'abc2@gmail.com'),
  (3, 'first', 'last 3', 'abc3@gmail.com');
  ```

Third, start the application, and spring boot creates this table on startup. Once the application is started you can go to this URL ![http://localhost:8080/h2-console](http://localhost:8080/h2-console) and access the database on the web browser. Make sure you have the same JDBC URL, username, and password as in the properties file.


