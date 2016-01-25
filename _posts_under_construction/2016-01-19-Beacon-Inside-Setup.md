---
layout: post
title:  "Beacon Inside set up"
date:   2016-1-19 13:00:00
categories: android, beacon,  
---
IN PROGRESS
# Beacon Inside set up
1. Download the beaconinside app.
2. Check that your beacons inside is working and that your phone can read and write in it.
3. Download the aar file
4.  Add `libs` in the repositories of gradle

```java

		repositories{
	    flatDir {
	        dirs 'libs'
	    }
	}
```

5. Add the aar in the dependencies

```java

	dependencies {
	...
		compile 'com.beaconinside:beaconinside-sdk-plain@aar'
	}
```

6. Init the Service in your **Main Activity**

```java

	BeaconService.init(this, "YOUR_TOKEN");
```

7. Get the token from  [https://manage.beaconinside.com/#/developers](https://manage.beaconinside.com/#/developers)

