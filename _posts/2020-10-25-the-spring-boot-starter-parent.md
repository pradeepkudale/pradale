---
layout: post
title:  "The Spring Boot Starter Parent"
date:   2020-10-25
categories: spring-boot
tags: spring-boot spring-boot-starter-parent
author: pradale
description: The spring boot starter parent can be used to manage project dependencies efficiently. It takes away the hassle to manage common properties and dependency versions in our application. 
---
The spring boot starter parent can be used to manage project dependencies efficiently. It takes away the hassle to manage common properties and dependency versions in our application.

Spring boot inherits all its dependencies from **spring-boot-dependencies** and if needed we can always override the properties provided by starter-parent pom. 

## How to Configure
We can configure a starter parent in two ways.

### Inheriting the Starter Parent POM

Configure to inherit from **spring-boot-starter-parent** as shown below.
```xml
<parent>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-parent</artifactId>
	<version>2.3.0.RELEASE</version>
</parent>
```

With the above configuration, we can override the dependencies present in our parent pom.xml file as follows.

```xml
<properties>
	<commons-pool.version>1.6</commons-pool.version>
	<hazelcast.version>3.12.7</hazelcast.version>
</properties>
```
To add a dependency that is not present in starter parent pom we have to add it under **dependencyManagement** or **dependencies** tag. If a dependency is added in these section then we have to mention it's version.

### The Dependency Management Tag
If we have our own parent pom file and still we want to have all the benefits of spring boot starter then we can add the **spring-boot-dependencies** dependency under the dependency management tag as shown below.
```xml
<dependencyManagement>
	<dependencies>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-dependencies</artifactId>
			<version>2.3.0.RELEASE</version>
			<type>pom</type>
			<scope>import</scope>
		</dependency>
	</dependencies>
</dependencyManagement>
```
>>import scope is only supported on a dependency of type `pom` in the `<dependencyManagement>` section.

## How to Exclude Transitive dependency
Transitive dependencies can be excluded from dependency as below.
```xml
<dependency>  
	<groupId>org.springframework</groupId>
	<artifactId>spring-core</artifactId>  
	<version>5.2.6.RELEASE</version>  
	<exclusions>  
		<exclusion>  
			<groupId>commons-logging</groupId>
			<artifactId>commons-logging</artifactId> 
		</exclusion>
	</exclusions>  
</dependency>
```
## Summary
In this tutorial, we saw how we can use the spring-boot-starter-parent artifact to manage our dependencies and override the properties.

Source code for this tutorial is available on [Github]()
