---
layout: post
title: "Don't use XIBS, use CALayers instead"
date: 2014-01-21 10:44:13 +0100
comments: true
categories: ios programming xib calayer
---

This tip is something that we use extensively here <a href=@"http://www.preplaysports.com/">@PrePlaySports</a>, in the iOS team.

First we don't use XIBs, ever (and <a href="http://www.youtube.com/watch?v=I5RqcYzrY4Y#t=2340">you shouldn't</a> either) ! So that means we have to deal with the view construction on the code side. The good thing with this approach is that it leaves us with great freedom as to how to build your view and it can be easily reviewed and merged.

So the main thing we would normally do is write all your `UIView` subclasses with other `UIView` subclasses (`UILabel`, `UIImageView`...). And this would work but as you view gets really big and if you need responsiveness, this is not the good approach.

Instead you should use, as much as possible, __`CALayers`__ instead of __`UIViews`__ to build your custom views.

### Using UIViews

Here is how you would probably write your custom `UIView` subclass:

``` objc MyCustomView using UIViews
#import "MyCustomView.h"

@interface MyCustomView ()
@property (nonatomic) UIImageView* myImageView;
@property (nonatomic) UILabel* myLabel;

- (void)initialize;

@end

@implementation MyCustomView

- (id)initWithFrame:(CGRect)frame
{
    self = [super initWithFrame:frame];
    if (self) {
        [self initialize];
    }
    return self;
}

- (void)initialize {
    // setup imageview
    self.myImageView = [[UIImageView alloc] initWithImage:[UIImage imageNamed:@"myImage"]];
    self.myImageView.contentMode = UIViewContentModeScaleAspectFit;
    
    // setup label
    self.myLabel = [[UILabel alloc] initWithFrame:CGRectZero];
    self.myLabel.backgroundColor = [UIColor clearColor];
    self.myLabel.font = [UIFont fontWithName:@"MyFontName" size:13];
    self.myLabel.textAlignment = NSTextAlignmentLeft;
    self.myLabel.textColor = [UIColor blackColor];
    self.myLabel.shadowColor = [UIColor whiteColor];
    self.myLabel.shadowOffset = (CGSize){0,-1.f};
    
    // add subviews
    [self addSubview:self.myImageView];
    [self addSubview:self.myLabel];
}

- (void)layoutSubviews {
    // layout you custom view here
}

@end

```

There is nothing fundamentally wrong with this code it will work fine and is pretty clean, but there are two things that can be improved: 

#### 1. Responsiveness

`UIViews` will always respond to touch events, always ! You might think that writting the following setters will prevent touch events to occur:

``` objc 
[self.myImageView setUserInteractionEnabled:NO];
[self.myLabel setUserInteractionEnabled:NO];
```

but you're dead wrong, the method `hitTest:withEvent:` will still be triggered on those `UIViews` elements. You will therefore loose performance if you need lots of reactivity on touch events.

#### 2. Animations

The other drawback with using `UIViews` subclasses is that when elements of the view gets udpated, there is no transtion from the previously rendered elements to the new one. Which is too bad since `UIView` use `CALayers` for rendering.

``` objc

// there will be no transition when you write this...
[self.myLabel setText:@"my custom text"];

// ... then later on change the text
[self.myLabel setText:@"my new custom text"];
```

### Using CALayers

Writing the same code but using `CALayers` requires just a few changes.

``` objc MyCustomView using CALayers
@interface MyCustomView ()
@property (nonatomic) CALayer* myImageLayer;
@property (nonatomic) CATextLayer* myTextLayer;

- (void)initialize;

@end

@implementation MyCustomView

- (id)initWithFrame:(CGRect)frame
{
    self = [super initWithFrame:frame];
    if (self) {
        [self initialize];
    }
    return self;
}

- (void)initialize {
    // setup imageview
    self.myImageLayer = [CALayer layer];
    self.myImageLayer.contents = (id)[UIImage imageNamed:@"myImage"].CGImage;
    self.myImageLayer.contentsScale = [[UIScreen mainScreen] scale];
    
    // setup label
    self.myTextLayer = [CATextLayer layer];
    self.myTextLayer.contentsScale = [[UIScreen mainScreen] scale];
    self.myTextLayer.backgroundColor = [UIColor clearColor].CGColor;
    self.myTextLayer.font = (__bridge CFTypeRef)[UIFont fontWithName:@"MyFontName" size:13];
    self.myTextLayer.fontSize = 13.f;
    self.myTextLayer.alignmentMode = kCAAlignmentLeft;
    self.myTextLayer.foregroundColor = [UIColor blackColor].CGColor;
    self.myTextLayer.shadowColor = [UIColor whiteColor].CGColor;
    self.myTextLayer.shadowOffset = (CGSize){0,-1.f};
    
    // add subviews
    [self.layer addSublayer:self.myImageLayer];
    [self.layer addSublayer:self.myTextLayer];
}

- (void)layoutSubviews {
    // layout you custom view here
}

@end
```