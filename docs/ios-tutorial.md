iOS Tutorial Based on the Standford 2013 course at standford.edu

#1 Basics Objective C 
*Card.h*
* public API
* who is the super class

```objective-c
 #import <Foundation/Foundation.h>

 @interface Card : NSObject

```

*Card.m* 
```objective-c
 #import "Card.h"

 @implementation Card

```  

###All objects are in the HEAP
Objectove C will allocate and free them

####strong
 Keep the object that this property points to in memory until I set this property to nil (zero)
(and it will stay in memory until everyone who has a strong pointer to it sets their property to nil too)

####weak
 If no one else has a strong pointer to this object, then you can throw it out of memory
and set this property to nil (this can happen at any time)

###Atomicity
####nonatomic
Is not thread safe.
Access to this property is not thread-safe”.
We will always specify this for object pointers in this course. If you do not, then the compiler will generate locking code that will complicate your code elsewhere

This line will make the next (synthesize) to be there but we wont see it in the code.(Is behind the scenes)

*Card.h*
```objective-c
@property (strong, nonatomic) NSString *contents;
```

###Properties: synthesize

This @synthesize is the line of code that actually creates the backing instance variable that is set and gotten.
￼Notice that by default the backing variable’s name is the same as the property’s name but with an underbar in front.

Properties: Getter and setters are accesed with dot notation.

This is the @property implementation that the compiler generates automatically for you (behind the scenes).
*Card.m*
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

###properties


default
*Card.h*
```objective-c
@property (nonatomic) BOOL chosen;
```
renamed
*Card.h*
```objective-c
@property (nonatomic, getter=isChosen) BOOL chosen;

```
###methods and sending messages
*Card.h*
```objective-c
- (int)match:(Card *)card;
```

*Card.m*
```objective-c
- (int)match:(Card *)card
{
int score = 0;
￼￼￼￼if ([card.contents isEqualToString:self.contents]) {
    score = 1;
	}
return score;
}
```

#2 Basics Objective C 2 

*Deck.h*
```objective-c
- (void)addCard:(Card *)card atTop:(BOOL)atTop;
- (void)addCard:(Card *)card; // Same without the parameter.
```
Arrays are inmutable, we need NSMutableArray.
*Decks.m*
```objective-c
@property (strong, nonatomic) NSMutableArray *cards; // of Card

###Initialize
Declaring a @property makes space in the instance for the￼￼pointer itself but does not
allocate space in the heap for the￼object the pointer points to.

All properties start out with a value of 0￼(called nil for pointers to objects).
So all we need to do is allocate and initialize the object if the pointer to it is nil.
This is called “lazy instantiation”.
Now you can start to see the usefulness of a @property


*Decks.m*
```objective-c
-(NSMutableArray *)cards
{
    if (!_cards) _cards = [[NSMutableArray alloc] init];
    return _cards;
}
```

###Syntetyze 2
if you implement BOTH the
setter and the getter for a property, then you have to create the instance variable for the property yourself.

If you implement only the setter OR the getter (or neither), the compiler adds this @synthesize for you.

*Decks.m*
```objective-c
@synthesize suit = _suit; // because we provide setter AND getter
```
###Class Methods
Usually these are either creation methods (like alloc or stringWithFormat:) or utility methods.

Class methods start with + Instance methods start with -.


###Initialization
Initialization in Objective-C happens immediately after allocation.￼
We always nest a call to init around a call to alloc e.g.Deck *myDeck = [[PlayingCardDeck alloc] init] or NSMutableArray *cards = [[NSMutableArray alloc] init].

￼Classes can have more complicated initializers than just plain “init" (e.g. initWithCapacity: or some such). We’ll talk more about that next week as well.

**Only call an init method immediately after calling alloc to make space in the heap for that new object. And￼never call alloc without immediately alling some init method on the newly allocated object.**

####instancetype
￼
It basically tells the compiler that this method returns an object which will be the same type as the object that this￼message was sent to.
We will pretty much only use it for init methods. Always use this return type for your init methods.
￼￼￼￼￼￼￼

