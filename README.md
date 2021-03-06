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

![](https://github.com/DrVicki/react-app-java-backend/blob/main/images/h2-in-mem-database.png)

Let’s add the repository files, service files, and entity classes as below and start the spring boot app.

**Users.java**
```
package com.bbtutorials.users.entity;

import javax.persistence.Column;
import javax.persistence.Entity;
import javax.persistence.Id;
import javax.validation.constraints.NotNull;


import lombok.Data;

@Entity
@Data
public class Users {
	
	@Id
	@Column
    private long id;

    @Column
    @NotNull(message="{NotNull.User.firstName}")
    private String firstName;
    
    @Column
    @NotNull(message="{NotNull.User.lastName}")
    private String lastName;
    
    @Column
    @NotNull(message="{NotNull.User.email}")
    private String email;

}
```

**UsersRepository.java**
```
package com.bbtutorials.users.repository;

import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.data.jpa.repository.JpaSpecificationExecutor;
import org.springframework.data.querydsl.QuerydslPredicateExecutor;
import org.springframework.data.rest.core.annotation.RepositoryRestResource;

import com.bbtutorials.users.entity.Users;

@RepositoryRestResource()
public interface UsersRepository extends JpaRepository<Users, Integer>, JpaSpecificationExecutor<Users>, QuerydslPredicateExecutor<Users> {}

```

**UsersService.java**
```
package com.bbtutorials.users.service;

import java.util.List;
import java.util.Random;

import org.springframework.stereotype.Component;

import com.bbtutorials.users.entity.Users;
import com.bbtutorials.users.repository.UsersRepository;

@Component
public class UsersService {
	
	private UsersRepository usersRepository;

    public UsersService(UsersRepository usersRepository) {
        this.usersRepository = usersRepository;
    }

    public List<Users> getUsers() {
        return usersRepository.findAll();
    }
    
    public Users saveUser(Users users) {
    	users.setId(new Random().nextInt());
    	return usersRepository.save(users);
    }

}
```

You can start the application in two ways: you can right-click on the ```UsersApplication``` and run it as a java application or do the following steps.

```
// mvn install
mvn clean install
// run the app
java -jar target/<repo>.war
```
![](https://github.com/DrVicki/react-app-java-backend/blob/main/images/start-springboot-app.gif)

Finally, you can list all the users with this endpoint http://localhost:8080/api/users.

![](https://github.com/DrVicki/react-app-java-backend/blob/main/images/java-port-8080.png)

## React App

Now the java code is running on port 8080. Now it’s time to look at the React app. The entire React app is under the folder ```src/main/ui```. You can create with this command ```npx create-react-app ui```. I am not going to put all the files in here. You can look at the entire [repo here](https://github.com/DrVicki/react-java-example).

Let’s see some important files here. Here is the service file which calls Java API.

**UserService.js**
```
export async function getAllUsers() {

    const response = await fetch('/api/users');
    return await response.json();
}

export async function createUser(data) {
    const response = await fetch(`/api/user`, {
        method: 'POST',
        headers: {'Content-Type': 'application/json'},
        body: JSON.stringify(data)
      })
    return await response.json();
}
```

Here is the app component which subscribes to these calls and gets the data from the API.


**App.js**
```
import React, { Component } from 'react';
import 'bootstrap/dist/css/bootstrap.min.css';
import './App.css';
import { Header } from './components/Header'
import { Users } from './components/Users'
import { DisplayBoard } from './components/DisplayBoard'
import CreateUser from './components/CreateUser'
import { getAllUsers, createUser } from './services/UserService'

class App extends Component {

  state = {
    user: {},
    users: [],
    numberOfUsers: 0
  }

  createUser = (e) => {
      createUser(this.state.user)
        .then(response => {
          console.log(response);
          this.setState({numberOfUsers: this.state.numberOfUsers + 1})
      });
      this.setState({user: {}})
  }

  getAllUsers = () => {
    getAllUsers()
      .then(users => {
        console.log(users)
        this.setState({users: users, numberOfUsers: users.length})
      });
  }

  onChangeForm = (e) => {
      let user = this.state.user
      if (e.target.name === 'firstname') {
          user.firstName = e.target.value;
      } else if (e.target.name === 'lastname') {
          user.lastName = e.target.value;
      } else if (e.target.name === 'email') {
          user.email = e.target.value;
      }
      this.setState({user})
  }

  render() {
    
    return (
      <div className="App">
        <Header></Header>
        <div className="container mrgnbtm">
          <div className="row">
            <div className="col-md-8">
                <CreateUser 
                  user={this.state.user}
                  onChangeForm={this.onChangeForm}
                  createUser={this.createUser}
                  >
                </CreateUser>
            </div>
            <div className="col-md-4">
                <DisplayBoard
                  numberOfUsers={this.state.numberOfUsers}
                  getAllUsers={this.getAllUsers}
                >
                </DisplayBoard>
            </div>
          </div>
        </div>
        <div className="row mrgnbtm">
          <Users users={this.state.users}></Users>
        </div>
      </div>
    );
  }
}

export default App;
```


## Interaction between Angular and Java API

In the development phase, the React app is running on port 3000 with the help of a ```create-react-app``` and Java API running on port 8080.
There should be some interaction between these two. We can proxy all the API calls to Java API. Create-react-app provides some inbuilt functionality and to tell the development server to proxy any unknown requests to your API server in development, add a proxy field to your ```package.json``` of the React. Have a look at the below ```package.json``` below. Remember you need to put this in the React UI ```package.json``` file.


**package.json**
```
{
  "name": "my-app",
  "version": "0.1.0",
  "private": true,
  "dependencies": {
    "@testing-library/jest-dom": "^4.2.4",
    "@testing-library/react": "^9.5.0",
    "@testing-library/user-event": "^7.2.1",
    "bootstrap": "^4.5.0",
    "react": "^16.13.1",
    "react-bootstrap": "^1.0.1",
    "react-dom": "^16.13.1",
    "react-scripts": "3.4.1"
  },
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test",
    "eject": "react-scripts eject"
  },
  "proxy": "http://localhost:8080",
  "eslintConfig": {
    "extends": "react-app"
  },
  "browserslist": {
    "production": [
      ">0.2%",
      "not dead",
      "not op_mini all"
    ],
    "development": [
      "last 1 chrome version",
      "last 1 firefox version",
      "last 1 safari version"
    ]
  }
}
```

With this in place, all the calls starting with ```/api``` will be redirected to http://localhost:8080 where the Java API is running.
Once this is configured, you can run the React app on port 3000 and java API on 8080 still make them work together.

```
// java API (Terminal 1)
mvn clean install
java -jar target/<war file name>
// React app (Terminal 2)
cd src/mamin/ui
npm start
```

## Summary

- There are many ways we can build React apps and ship them for production.
- One way is to build React with Java.
- In the development phase, we can run React and Java on separate ports.
- The interaction between these two happens with proxying all the calls to API.
- In the production phase, you can build the React app and put all the assets in the build folder and load it with the java code.
- We can package the application for production with a maven plugin and gulp.
