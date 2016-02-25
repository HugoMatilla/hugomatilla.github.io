---
layout: post
title:  "Simple use of up navigation"
date:   2016-02-25 18:00:00
categories: tutorial
image : main-up-navigation.jpg
---

[Providing Up Navigation](http://developer.android.com/intl/es/training/implementing-navigation/ancestral.html) is an android design concept that we should follow. and it is easy to use, so why don't give it a try.

## Simple use
It is easy to use, just  need this in your manifest:

```java

	   <activity
            android:name=".ui.activities.DetailActivity"
            android:parentActivityName=".ui.activities.MainActivity" > //<---Only add this
            <meta-data // This if you need to target Android versions lower than 16
                android:name="android.support.PARENT_ACTIVITY"
                android:value="com.antonioleiva.weatherapp.ui.activities.MainActivity" />
        </activity>

```

Add `android:parentActivityName="parentActivityName"` to the child activity in your manifest and you are up to use it. 

It will add the back arrow to your child activity and when clicked it will go to the parent activity.

You don't need to write this anymore. 

```java

	getActionBar().setDisplayHomeAsUpEnabled(true);

	@Override
	public boolean onOptionsItemSelected(MenuItem item) {
	    switch (item.getItemId()) {
	    // Respond to the action bar's Up/Home button
	    case android.R.id.home:
	        // Code to go to the parent activity
	        return true;
	    }
	    return super.onOptionsItemSelected(item);
	}
```

You will get the same functionality for free.

## The problem (parent activity recreation)
But one important think that I missed the first time I tried it, any time that you go to the parent activity using the up navigation (back arrow), the parent activity will be recreated, i.e. if it is a list you will go to the top element, if the parent activity has complex UI and images all if it will have to be recreated, worse, if the parent activity got some data from the Internet that was not cached, the whole download process will be done again. It is easy to fix this.

## Why is this happening
It is explained here [Providing Up Navigation](http://developer.android.com/intl/es/training/implementing-navigation/ancestral.html):

* If the parent activity has launch mode <singleTop>, or the up intent contains FLAG_ACTIVITY_CLEAR_TOP, **the parent activity is brought to the top of the stack**, and receives the intent through its onNewIntent() method.
* If the parent activity has launch mode <standard>, and the up intent does not contain FLAG_ACTIVITY_CLEAR_TOP, **the parent activity is popped off the stack, and a new instance of that activity is created** on top of the stack to receive the intent.

## Solution
Just add `launchMode="singleTop` to the parent activity.

```java

	<activity android:name=".ui.activities.MainActivity"
                  android:launchMode="singleTop">
    </activity>
```

## Conclusion 	
This is a small article about a simple problem in a big feature. Up navigation and launch modes are a big topic, so be careful when you use them and read the documentation understand better what happens behind the scenes