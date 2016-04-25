---
layout: post
title:  "Snippets for testing in Kotlin"
date:   2016-1-4 22:28:00
categories: test,kotlin, android
image : main-testing.jpg
---

## Some simple snippets to do logic testing in kotlin

#JUnit Test
When doing Unit testing we don't depend on Android so here is the sample of a basic test.

```kotlin

	package com.hugomatilla.starwars.data

	import com.google.gson.Gson
	import com.google.gson.JsonSyntaxException
	import org.junit.Assert.*
	import org.junit.Test
	import java.io.File
	import java.net.MalformedURLException


	/**
	 * Created by hugomatilla on 02/03/16.
	 */
	class CloudUnitTest {
	    // ----- Article List -----
	    @Test
	    fun requestTopArticles() {
	        val articles = RestService().fetchArticlesList()
	        assertEquals(articles.size, 245)
	    }

	    // ------ Exceptions -----
	    @Test
	    fun catchMalformedUrl() {
	        try {
	            RestService().fetchArticlesList("htp://google.com)
	            fail("MalformedURLException not catch");
	        } catch (error: MalformedURLException) {
	            assert(true)
	        }
	    }

	    // ----- Article Sections -----
	    @Test
	    fun requestArticleSections() {
	        val sections = RestService().fetchArticleSections(1)
	        if (sections != null)
	            assertEquals(sections.elementAt(0).title, "Section Title")
	        else
	            fail("Sections is null")
	    }
	}
```

#Android Tests
If we need a context for example to access the data base we will need to use android tests instead of "normal" tests.

We do not need any UI testing, just the context, so we don't need to import espresso libraries for this test. 
So our dependencies in the build.gradle file will need the runner and the rules.

```java

	//test
    testCompile 'junit:junit:4.12'
    //instrumentation tests
    androidTestCompile "com.android.support.test:runner:0.4.1"
    androidTestCompile "com.android.support.test:rules:0.4.1"
```

Other thing that we might need is an application where the android test will "take" the context. This is easy if we are making the tests for the presentation layer ([Clean Architecture](https://blog.8thlight.com/uncle-bob/2012/08/13/the-clean-architecture.html)). In this case I am making a test for the data layer, that is a different module and does not have any idea of what is in the presentation layer. So I can not import any activity or my "MyApplication.class" to the test.
What you can do is use the `Application.class`. The runner will create it before running the test, and use its context.

A test of the DataBase will look something like this.

```kotlin

	class DataTests : ApplicationTestCase<Application>(Application::class.java) {
	    private var DB: Db? = null

	    @Throws(Exception::class)
	    override fun setUp() {
	        super.setUp()
	        createApplication()
	        DB = Db(DbHelper(application.applicationContext, DbHelper.DB_NAME_MOCK, 1), DbMapper())
	    }

	    fun test_saveArticle() {
	        val article = ArticleDomain(1, "Title", "Abstract", "Thumbnail", 1, 2, "Url", "Type", emptyList())
	        DB!!.saveArticle(article)
	        val result = DB!!.getArticleDetailById(1)
	        if (result != null)
	            assertEquals(result, article)
	        else
	            fail("Return value from the data base is null")
	    }


	    override fun tearDown() {
	        DB!!.clearDatabase()
	        super.tearDown()
	    }
	}

```
Remember that tests must start with the prefix `test_` as in `test_saveArticle()`

To create my database instance I need to pass to the constructor a Database Helper with a context, a name of the database (I use a mock name for the tests) and a version number, and a data mapper.

And don't forget to clear the database in the tear down of the test, so every test will start with an fresh and empty database.

You can find more in this [github project](https://github.com/HugoMatilla/StarWars-TheKotlinAwakens)

