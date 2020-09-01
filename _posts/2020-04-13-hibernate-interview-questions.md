---
layout: post
title:  "Hibernate interview questions"
image: ''
date:   2020-04-13 11:30:00 +0800
tags:
- hibernate
description: ''
categories:
- Learn Hibernate
---


**1) What is hibernate ?**

Hibernate is a Java framework that simplifies the development of Java application to interact with the database. It is an open source, lightweight, ORM (Object Relational Mapping) tool. Hibernate implements the specifications of JPA (Java Persistence API) for data persistence.

**2) What is JPA?**

Java Persistence API (JPA) is a Java specification that provides certain functionality and standard to ORM tools. The javax.persistence package contains the JPA classes and interfaces.


**3) What are the key components/objects of hibernate?**

Following are the key components/objects of Hibernate −

* **Configuration** 

An instance of Configuration allows the application to specify properties and mapping documents to be used when creating a SessionFactory. Usually an application will create a single Configuration, build a single instance of SessionFactory and then instantiate Sessions in threads servicing client requests. The Configuration is meant only as an initialization-time object. SessionFactorys are immutable and do not retain any association back to the Configuration.

A new Configuration will use the properties specified in hibernate.properties by default.

* **SessionFactory** − 

Configuration object is used to create a SessionFactory object which in turn configures Hibernate for the application using the supplied configuration file and allows for a Session object to be instantiated. The SessionFactory is a thread safe object and used by all the threads of an application.

The SessionFactory is a heavyweight object; it is usually created during application start up and kept for later use. You would need one SessionFactory object per database using a separate configuration file. So, if you are using multiple databases, then you would have to create multiple SessionFactory objects.


* **Session** 

A Session is used to get a physical connection with a database. The Session object is lightweight and designed to be instantiated each time an interaction is needed with the database. Persistent objects are saved and retrieved through a Session object.

The session objects should not be kept open for a long time because they are not usually thread safe and they should be created and destroyed them as needed.

* **Transaction** 

A Transaction represents a unit of work with the database and most of the RDBMS supports transaction functionality. Transactions in Hibernate are handled by an underlying transaction manager and transaction (from JDBC or JTA).

This is an optional object and Hibernate applications may choose not to use this interface, instead managing transactions in their own application code.

* **Query** 

Query objects use SQL or Hibernate Query Language (HQL) string to retrieve data from the database and create objects. A Query instance is used to bind query parameters, limit the number of results returned by the query, and finally to execute the query.

* **Criteria** − Used to create and execute object oriented criteria queries to retrieve objects.

Criteria objects are used to create and execute object oriented criteria queries to retrieve objects.

**4) What is difference between getCurrentSession() and openSession() in Hibernate? **


|       Parameter     |                                openSession                                 |                                                    getCurrentSession                                                     |
|---------------------|----------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------|
|   Session  creation | Always open new session                                                    | It opens a new Session if not exists , else use same session which is in current hibernate context.                      |
|   Session close     | Need to close the session object once all the database operations are done | No need to close the session. Once the session factory is closed, this session object is closed.                         |
|   Flush and close   | Need to explicity flush and close session objects                          | No need to flush and close sessions , since it is automatically taken by hibernate internally.                           |
|   Performance       | In single threaded environment , it is slower than getCurrentSession       | In single threaded environment , it is faster than openSession                                                           |
|   Configuration     | No need to configure any property to call this method                      | Need to configure additional property:</br> <property name=""hibernate.current_session_context_class"">thread</property> |


**5) What are different types of caches available in Hibernate?**

* First-level cache

The first-level cache is the Session cache and is a mandatory cache through which all requests must pass. The Session object keeps an object under its own power before committing it to the database.

* Second-level cache

Second level cache is an optional cache and first-level cache will always be consulted before any attempt is made to locate an object in the second-level cache. The second-level cache can be configured on a per-class and per-collection basis and mainly responsible for caching objects across sessions.

EHCache (org.hibernate.cache.EhCacheProvider)
OSCache (org.hibernate.cache.OSCacheProvider)
SwarmCache (org.hibernate.cache.SwarmCacheProvider)
JBoss TreeCache (org.hibernate.cache.TreeCacheProvider)

* Query level cache

Hibernate also implements a cache for query resultsets that integrates closely with the second-level cache.

This is an optional feature and requires two additional physical cache regions that hold the cached query results and the timestamps when a table was last updated. This is only useful for queries that are run frequently with the same parameters.

**5) get() vs load() method?**

  
| Sl No |          Key           |                                                        get()                                                         |                                   load()                                   |
|-------|------------------------|----------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------|
|     1 | Basic                  | It  is used to fetch data from the database for the given identifier                                                 | It  is also used to fetch data from the database for the given identifier  |
|     2 | Null Object            | It object not found for the given identifier then it will return null object                                         | It will throw object not found exception i.e ObjectNotFoundException       |
|     3 | Lazy or Eager loading  | It returns fully initialized object so this method eager load the object                                             | It always returns proxy object so this method is lazy load the object      |
|     4 | Performance            | It is slower than load() because it return fully initialized object which impact the performance of the application  | It is slightly faster.                                                     |
|     5 | Use Case               | If you are not sure that object exist then use get() method                                                          | If you are sure that object exist then use load() method                   |


Example:

If you are trying to load /get Empoyee object where empid=20. But assume that record is not available in database.

  
  ```Employee employee1 = session.load(Employee.class,20);  //Step-1```
  
  ```System.out.println(employee1.getEmployeeId();       //Step-2  --Output =20```
  
  ```System.out.println(employee1.getEmployeeName();       //Step-3 -->Output =ObjectNotFoundException ```
  
If you use load in step-1 hibernate won't fire any select query to fetch employee record from the database at this moment. At this point hibernate gives a dummy object (Proxy). This dummy object doesnt contain anything. It is new Employee(20). you can verify this in step-2 it will print 20. but in step-3 we are trying to find employee information. so at this time hibernate fires a sql query to fetch Empoyee objectr. If it is not found in DB.throws ObjectNotFoundException.

  ```Employee employee2 = session.get(Employee.class,20);  //Step-4  ```
  
For session.get() hibernate fires a sql query to fetch the data from db. so in our case id=20 not exists in DB. so it will return null.
