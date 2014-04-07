---
layout:     post
title:      "Persisting managed objects with scalar attributes"
date:       2010-10-30 21:54:00
comments:   true
---

Core Data natively supports attributes that are of type NSString, NSNumber, or NSData. You can however use other types with a bit of extra work. If you have an attribute that is a scalar value such as BOOL, you can have your managed object persist it by first converting it to a NSNumber. For example, CheckBox is a NSManagedObject with an attribute called checked that is of type BOOL. BOOL in Objective-C can easily be converted to and from a NSNumber.

The header file contains the BOOL attribute for CheckBox.

{% highlight objective-c %}
// CheckBox.h
#import <CoreData/CoreData.h>

@interface CheckBox : NSManagedObject {
}
@property(nonatomic) BOOL checked;

@end
{% endhighlight %}

The implementation file contains a PrimitiveAccessors category for the underlying primitiveChecked value, which stores the checked value as a NSNumber. We then override the accessors and mutators for the checked attribute to convert the BOOL value to and from a NSNumber.

{% highlight objective-c %}
// CheckBox.m
#import "CheckBox.h"

@interface CheckBox (PrimitiveAccessors)
@property (nonatomic, retain) NSNumber *primitiveChecked;
@end

@implementation CheckBox

- (BOOL)checked {
    [self willAccessValueForKey:@"checked"];
    BOOL isChecked = [[self primitiveChecked] boolValue];
    [self didAccessValueForKey:@"checked"];
    return isChecked;
}

- (void)setChecked:(BOOL)isChecked {
    [self willChangeValueForKey:@"checked"];
    [self setPrimitiveChecked:[NSNumber numberWithBool:isChecked]];
    [self didChangeValueForKey:@"checked"];
}

@end
{% endhighlight %}

