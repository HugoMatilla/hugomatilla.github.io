---
layout: post
title:  "Wannabe"
date:   2016-1-12 17:50:00
categories: architecture
---
IN PROGRESS

*Questions and decisions about software design.*

# 1 Cloud & DataBase
*1. Should the data be saved in the cloud classes or in different ones?
	Different ones. The more decouple the better. Cloud should do one thing get data from the cloud and pass it, no less no more.
*2. Should the cloud classes have access to the database? 
	Answered in #1. Why would the cloud want to save the data.
*3. When should the data be stored?
	Whenever we want, it is a business question.
*4. Cloud layer uses cloud classes, database uses database classes, domain layer uses domain classes. Has sense that data goes from cloud to domain, from domain database, and from database to domain again?
	As in #3 it is a business question how we want the the data to go to the business logic. 
	Directly from the cloud, after saving it in the database, or save it only in the database and then read the data base.

# 2 How to generalize the use of anko async and uiThread to all use cases. ie how to make and abstract class that do that.

#3 How are errors propagated from the data layer to the presentation layer

#4 If a mapper needs data from the data base who should provide this data ie: Person from DB, but person has "former jobs", where and how to get this data, in the mapper in the db before the mapper?

#5 If in the login page I have to make extra login, like remove old data from the data base how I do it? Is a new presenter, use case?

#6 A selection page: choose between 2 views like log in and sign up, should I implement this as an mvp, or it is only the v? 