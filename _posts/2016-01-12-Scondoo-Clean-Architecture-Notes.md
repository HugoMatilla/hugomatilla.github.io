---
layout: post
title:  "Scondoo Clean Architecture Notes"
date:   2016-1-12 17:50:00
categories: architecture
---

Just some notes to rememeber myself in the future. I already build the project and now 1 month later i will try to rememember what I did.

The project is bases on http://fernandocejas.com/2014/09/03/architecting-android-the-clean-way/

**3 modules / layers.**

* Data: Androidn dependent
* Domain: Androidn independent
* Presentation: Androidn dependent, MVP

Packaging by feature: high cohesion and high modularity, minimal coupling between packages. A perfect scenario would be to delete a feature package in each layer and nothing else would be affected.

Starting from the bottom up,not really, just as shown in the projct structure of Android studio :) I will structure it later.

# LAYERS

## PRESENTATION
Folders:

* exception
* mvp
* navigation

Files:

* ScondooApplication (App libraries initialization)

### Navigation Folder

Navigator.java
	To navigate through the app.

### MVP Folder

#### Navigator feature
Contains the functionallity of the Android Navigator (Side Menu)
Has the Activity and the fragments of the menu.




