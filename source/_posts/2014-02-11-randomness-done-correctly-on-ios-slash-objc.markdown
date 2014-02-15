---
layout: post
title: "Why you should not do modulo bias for randomness"
date: 2014-02-11 13:59:36 +0100
comments: true
categories: ios randomness programming
---

While reading a great article on [NSHipster](http://nshipster.com/) about [randomness](http://nshipster.com/random/), they suggested to use `arc4random_uniform()` instead of using the remainder of a `arc4random()` call. 

The article simply points to another site for explaining why we should use this method for [modulo bias](http://eternallyconfuzzled.com/arts/jsw_art_rand.aspx), but the article itself doesn't explain clearly why using the remainder `%` (or also called modulo bias) is bad behavior.

It only says that using the `%` only works if `n` devides `RAND_MAX` and here is why:

Imagine that `RAND_MAX` is equal to 11 and you want a random number between 0 and 4. You would write: 

``` c
int r = arc4random()%5;
```

And you would indeed get a random number between 0 & 5. You would have more chances to have 0 and 1 than 2,3 or 4. And here is why:

<table class="code">
<tr><td>arc4random()</td><td>0</td><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td><td>6</td><td>7</td><td>8</td><td>9</td><td>10</td><td>11</td></tr>
<tr><td>arc4random()%5</td><td>0</td><td>1</td><td>2</td><td>3</td><td>4</td><td>0</td><td>1</td><td>2</td><td>3</td><td>4</td><td>0</td><td>1</td></tr>
</table>

So the probabily of having 0 is equal to 1/4 while the probabily of having 2 is 1/6. Therefore you loose [uniform distribution](http://en.wikipedia.org/wiki/Uniform_distribution_\(discrete\)).

You'll also notice that if `n` devides `RAND_MAX` then, the distribution is uniform again.

<table class="code">
<tr><td>arc4random()</td><td>0</td><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td><td>6</td><td>7</td><td>8</td><td>9</td><td>10</td><td>11</td></tr>
<tr><td>arc4random()%12</td><td>0</td><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td><td>6</td><td>7</td><td>8</td><td>9</td><td>10</td><td>11</td></tr>
</table>


The proper way of generating a random number between 0 & 4 on iOS is to use the function `arc4random_uniform()`

``` c
// random number between 0 & n
int r = arc4random_uniform(n);
// random number between n & m;
int r = n + arc4random_uniform(m - n);
```

Also if you look at the value of `RAND_MAX` on iOS you would see:

``` objc
// RAND_MAX = 2147483647 = 2^31 − 1
#define RAND_MAX  0x7fffffff
```

`RAND_MAX` is the [8th Mersenne prime number](http://en.wikipedia.org/wiki/2147483647), so there is no way that you could ever take a value of `n` that nicely devices `RAND_MAX`.

