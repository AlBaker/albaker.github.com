---
layout: post
title: "Command Line App with Spring Boot"
description: "Convenience packaging with Spring Boot"
category:
tags: []
---
{% include JB/setup %}

Recently I had the need to bundle an application to run on the command line.  Before, I had created a war file with embeddd Jetty, so that all the normal dependencies could just be internally dropped in WEB-INF/lib.  There's a new way to go about this type of application with [Spring Boot](http://projects.spring.io/spring-boot/).

Whether you use the Spring framework, or even Spring Boot or not, the Spring Boot build file and package rewrite they do to support, effectively, jars-in-jars classloading is extremely usefl.

Here's all you need.  Your main class doesn't have to do anything special, you package a jar with the application plugin as normal, and that's that.


```
buildscript {
  repositories {
    maven { url "http://repo.spring.io/libs-snapshot" }
    mavenLocal()
  }
  dependencies {
  classpath("org.springframework.boot:spring-boot-gradle-plugin:1.0.2.RELEASE")
  }
}

apply plugin: 'groovy'
apply plugin: 'eclipse'
apply plugin: 'application'
apply plugin: 'spring-boot'


// ETL or Client
mainClassName = "your.mainClass"


repositories {
  mavenLocal()
  mavenCentral()
  maven { url "http://repo.spring.io/snapshot" }
  maven { url "http://repo.spring.io/milestone" }
}

dependencies {

  // your dependencies
  compile("org.springframework.boot:spring-boot-starter")
  testCompile("org.springframework.boot:spring-boot-starter-test")
}

jar {
  manifest {
    attributes 'Implementation-Title': 'your-title',
    'Implementation-Version': version,
    'Built-By': System.getProperty('user.name'),
    'Built-Date': new Date(),
    'Built-JDK': System.getProperty('java.version'),
    'Main-Class': mainClassName
  }
}

```


Enjoy!

