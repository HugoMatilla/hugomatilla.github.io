iOS Tutorial

#1
class.h 
* public API
* who is the super class

```objective-c
 #import <Foundation/Foundation.h>
 @interface Card : NSObject

```

class.m: 

```objective-c
 #import "Card.h"

 @implementation Card

```  

All objects are in the HEAP
* objC will allocate and free them

** strong :keep the object that this property points to
in memory until I set this property to nil (zero)
(and it will stay in memory until everyone who has a strong pointer to it sets their property to nil too)

** weak: if no one else has a strong pointer to this object, then you can throw it out of memory
and set this property to nil
(this can happen at any time)

```objective-c
@property (strong, nonatomic) NSString *contents;