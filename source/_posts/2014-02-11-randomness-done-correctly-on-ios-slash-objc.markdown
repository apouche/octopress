---
layout: post
title: "randomness done correctly on iOS/ObjC"
date: 2014-02-11 13:59:36 +0100
comments: true
categories: ios randomness programming
---

While reading a great article on [NSHipster](http://nshipster.com/) about [randomness](http://nshipster.com/random/), they suggested to use `arc4random_uniform()` instead of using the remainder of a `arc4random()` call. 

The article simply points to another site for explaining why we should use this method for [modulo bias](http://eternallyconfuzzled.com/arts/jsw_art_rand.aspx), but the article itself doesn't explain clearly why using the remainder `%` is bad behavior.

It says that using the `%` only works if `n` devides `RAND_MAX` and here is why:

Imagine that `RAND_MAX` is equal to 11 and you want a random number between 0 and 4. You would write: 

``` c
int r = arc4random()%5;
```

And you would indeed get a random number between 0 & 5. You would have more chances to have 0 and 1 than 2,3 or 4. And here is why:

<table class="code">
<tr><td>rand()</td><td>0</td><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td><td>6</td><td>7</td><td>8</td><td>9</td><td>10</td><td>11</td></tr>
<tr><td>r</td><td>0</td><td>1</td><td>2</td><td>3</td><td>4</td><td>0</td><td>1</td><td>2</td><td>3</td><td>4</td><td>0</td><td>1</td></tr>
</table>

0x7FFFFFFF = 2147483647