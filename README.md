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

