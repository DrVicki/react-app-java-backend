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


