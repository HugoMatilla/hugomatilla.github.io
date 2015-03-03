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

#####All objects are in the HEAP
objC will allocate and free them

######strong
 Keep the object that this property points to in memory until I set this property to nil (zero)
(and it will stay in memory until everyone who has a strong pointer to it sets their property to nil too)

######weak
 If no one else has a strong pointer to this object, then you can throw it out of memory
and set this property to nil (this can happen at any time)

#####Atomicity
######nonatomic
Is not thread safe.
Access to this property is not thread-safe”.
We will always specify this for object pointers in this course. If you do not, then the compiler will generate locking code that will complicate your code elsewhere

```objective-c
@property (strong, nonatomic) NSString *contents;
```

#####synthesize

This @synthesize is the line of code that actually creates the backing instance variable that is set and gotten.
￼Notice that by default the backing variable’s name is the same as the property’s name but with an underbar in front.


This is the @property implementation that the compiler generates automatically for you (behind the scenes).
```objective-c
@synthesize contents = _contents;

- (NSString *)contents
{
    return _contents;
}
- (void)setContents:(NSString *)contents
{
    _contents = contents;
}
```

