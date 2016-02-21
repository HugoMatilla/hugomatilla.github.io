---
layout: post
title:  "Use IdlingResource to make your tests wait"
date:   2016-02-21 18:00:00
categories: tutorial
image : main-idling.jpg
---

## _Small smaple to show how to run tests, after your application has gotten all data needed from the backend._

I just started with Espresso, I was following this nice tutorial, [Testing a sorted list with Espresso](http://blog.egorand.me/testing-a-sorted-list-with-espresso/) to test if the elements on a listview are sorted alphabetically. I thought it was a good idea to use it in an app I am making for leraning purpouses.
The article is easy to follow and when I was waiting for the _"All test passed"_ message a I got a disappponted. The list a was testing was **null**. Sad face :(

Of course it was null, the test runs right away after the activity starts, and the data I am using to inflate my list comes from the internet, so if I want to test this data, the first thing I have to do is wait for the data to be downloaded.

**NOTE:** I am new in testing so everything I have done so far is just for learning the use of Espresso, so probably test if the order is shown alphabetically is something that you won't do directly in the activity, and checking if the data is synced is probably better to do it in the thread that handles the downloads and not in the activity itself.

## What is an Idling Resource
The best explanation is in the [Android Documetatation](http://developer.android.com/intl/es/reference/android/support/test/espresso/IdlingResource.html)
*...test authors can register the custom resource and Espresso will wait for the resource to become idle prior to executing a view operation.*

So it is a resource like an anctivity or a service, that the test will wait to be run, until some condition to happend. 

## Creating the test
This will be the basic structure for our test. If you need more info just check the article above mentioned:[Testing a sorted list with Espresso](http://blog.egorand.me/testing-a-sorted-list-with-espresso/)

{% highlight java %}

	@RunWith(AndroidJUnit4.class)
	@LargeTest
	public class MainActivityTest {

	    @Rule
	    public ActivityTestRule<MainActivity> activityTestRule = new ActivityTestRule<>(MainActivity.class);


	    @Test
	    public void testToBeRunAfterSync() {
	        //... 
	    }
	}
{% endhighlight %}

If we use this template, `testToBeRunAfterSync()` will be run as soon as the acticvity is visible. We want to wait until we have data in the activity, so we need to tell the test to wait for that.

## Implementing the IdlingResource

We will create a class that implements the `IdlingResource` interface. Here we will say when our resource (MainActivity) is idle (ready to be tested). This will be done in `isIdleNow()` create the logic you need to know when it happends. In this case I have a parameter in `MainActivity` that is set to true when the data is synced. I access it bia `activity.isSyncFinished()`

{% highlight java %}
	
	public class MainActivityIdlingResource implements IdlingResource {

	    private MainActivity activity;
	    private ResourceCallback callback;

	    public MainActivityIdlingResource(MainActivity activity) {
	        this.activity = activity;
	    }

	    @Override
	    public String getName() {
	        return "MainActivityIdleName";
	    }

	    @Override
	    public boolean isIdleNow() {
	        Boolean idle = isIdle();
	        if (idle) callback.onTransitionToIdle();
	        return idle;
	    }

	    public boolean isIdle() {
	        return activity != null && callback != null && activity.isSyncFinished();
	    }

	    @Override
	    public void registerIdleTransitionCallback(ResourceCallback resourceCallback) {
	        this.callback = resourceCallback;
	    }
	} 
{% endhighlight %}

## Register the IdlingResource to our test
At this point we need to add the register and unregister methods to our test class.

{% highlight java %}

	@RunWith(AndroidJUnit4.class)
	@LargeTest
	public class MainActivityTest {

		private MainActivityIdlingResource idlingResource;

	    @Rule
	    public ActivityTestRule<MainActivity> activityTestRule = new ActivityTestRule<>(MainActivity.class);
	
		@Before
	    public void registerIntentServiceIdlingResource() {
	        MainActivity activity = activityTestRule.getActivity();
	        idlingResource = new MainActivityIdlingResource(activity);
	        Espresso.registerIdlingResources(idlingResource);
	    }

	    @After
	    public void unregisterIntentServiceIdlingResource() {
	        Espresso.unregisterIdlingResources(idlingResource);
	    }


	    @Test
	    public void testToBeRunAfterSync() {
	        //... 
	    }
	}
{% endhighlight %}

Now the test `testToBeRunAfterSync()` will be run after we call `callback.onTransitionToIdle()` on the `MainActivityIdlingResource`

## Conclusion

Implementing `IdlingResource` allows us to be in charge of when the tests run, so we have the control on  what and how to wait for before we run them. This is particullary helpful when our application does some  tasks that take long times before it is testeable, as getting data from cloud sources. 

This is more a snippet as a explanation of how the `idlingResource` works. For more detailed information read the [Android Documetatation](http://developer.android.com/intl/es/reference/android/support/test/espresso/IdlingResource.html) and this two articles that covers this topic in more detail.

Chiu-Ki Chan @ Square Island: [Espresso: Custom Idling Resource](http://blog.sqisland.com/2015/04/espresso-custom-idling-resource.html)

Stefano Dacchille @ Jimbo Blog: [Wait for it...a deep dive into Espresso's Idling Resources](http://dev.jimdo.com/2014/05/09/wait-for-it-a-deep-dive-into-espresso-s-idling-resources/)


