title: NSBundle's pathForResource won't fail fast
date: 2011-11-28 09:34:00
tags: [Apple,fail fast,iOS,lean]
---

One of the base ideas of lean software development is the concept of _failing fast._ This means that once a software element has determined that it cannot do it's job correctly, given the input it has, it should fail immediately and **loudly**. This will force the user to check the input and fix his mistake.  

Apple's Foundation.framework's NSBundle class has a nice example of how to do completely ignore this concept. For example, consider this code:  

>     NSLog(@"Path with real file name: %@", [[NSBundle mainBundle] pathForResource:**@"test"** ofType:@"png"]);  
>     NSLog(@"Path with nil file name: %@", [[NSBundle mainBundle] pathForResource:**nil** ofType:@"png"]);

What do you except it to print in the log, assuming the bundle contains the file 'test.png'? Nil string? Some kind of exception or error?  

Actually, it prints this:  

> 2011-11-28 10:32:42.733 Test[666:40b] Path with real file name: /Users/rkovacevic/Library/Application Support/iPhone Simulator/5.0/Applications/B12B5FC0-6DDA-4908-AB05-1B07A74341E7/Test.app/test.png  
> 2011-11-28 10:32:42.735 Test[666:40b] Path with nil file name: /Users/rkovacevic/Library/Application Support/iPhone Simulator/5.0/Applications/B12B5FC0-6DDA-4908-AB05-1B07A74341E7/Test.app/test.png

If you pass a 'nil' string to 'pathForResource' it will just find the first file of the given type, and pass it's path to you. Beware of this, or you will spend hours looking for a bug, assuming that it can't possibly be that the NSString you passed to 'pathForResource' is 'nil', because it wouldn't just make up a path and return it to you. Well, actually, it would, and it does. With Apple - assume nothing.