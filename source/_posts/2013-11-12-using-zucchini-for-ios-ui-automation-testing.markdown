---
layout: post
title: "Using Zucchini for iOS7 UI automated tests"
date: 2013-11-12 17:23
comments: true
categories: ios programming testing
---
If, like me, you have always been quite doubtful about ios UI automated tests, you can rejoice because the new framework zucchini is a great tool for the job.

It all started after this <a href="https://speakerdeck.com/voxdolo/ios-bdd-beatdown">excellent ios test tools comparison</a>. In this comparison there are two frameworks that take the spotlight, <a href="http://calaba.sh/">calabash</a> and <a href="http://www.zucchiniframework.org/">zucchini</a>.

While I tried calabash, it just didn't workout but this will be described in another post.

<h3>Installation</h3>
Given that you installed <a href="http://brew.sh/">homebrew</a>, the setup of zucchini is pretty straightforward.

```
brew update 
brew install imagemagick npm
npm install -g coffee-script
```

don't forget the `-g` option to `npm install` otherwise it will be a local installation.

<h3>Setting up a simple XCode project</h3>
Create a basic master/view application for ios.
<img src="/images/posts/zucchini/1.png" />

<h3>Create the zucchini hierarchy</h3>
Zucchini uses the files located in a specific folder (usually called `features`). The config file is located in `features/support/config.yml` and all other tests scenario are located in `features/other-folders` where other-folders is anything but support.

```bash
mkdir -p features/support
touch features/support/config.yml
```

Edit the `features/support/confing.yml` and write

```yml
app: './build/testZucchini.app'

devices:
  Simulator:
    screen: retina_ios7
    default: true
    simulator: iPhone (Retina 4-inch)
```

The `screen` parameter will be used both as the default mask name and the folder where to write the test scripts.

<h3>Writing your first script</h3>
Because, at the time of this writing, zucchini is not yet fully compliant with ios7, a few more steps are required to complete the setup.

```
zucchini generate --feature features/simple-test
mkdir -p features/simple-test/reference/retina_ios7
mkdir -p features/support/screens
mkdir -p features/support/masks

touch features/support/screens/master.coffee
```

As mentioned in the previous section the folder `reference/retina_ios7` will be used by zucchini as the reference folder where it will compare the screenshots taken by the application and the one the application should look like.

Next step, edit the `features/simple-test/feature.zucchini` file and write

```
Start on the "Master" screen:
  Take a screenshot named "master_screen"
```

and the `features/support/screens/master.coffee` file

```javascript
class MasterScreen extends Screen
  anchor: -> $("navigationBar[name=Master]")

  constructor: ->
    super 'master'
```

That's it you can now run ```zucchini run features``` and it should now work.

```
> zucchini run features                                                                                                                                           [14:22:07]
2013-11-12 14:23:18.907 ScriptAgent[10497:2f07] CLTilesManagerClient: initialize, sSharedTilesManagerClient
2013-11-12 14:23:18.908 ScriptAgent[10497:2f07] CLTilesManagerClient: init
2013-11-12 14:23:18.908 ScriptAgent[10497:2f07] CLTilesManagerClient: reconnecting, 0x978a690
2013-11-12 13:23:19 +0000 Debug: target.setDeviceOrientation("1")
2013-11-12 13:23:19 +0000 Default: Found anchor for screen 'Master'
2013-11-12 13:23:19 +0000 Default: Screenshot of screen 'master' taken with orientation 'Portrait'
2013-11-12 13:23:19 +0000 Debug: target.captureRectWithName("{origin:{x:0.00,y:0.00}, size:{height:568.00,width:320.00}}", "01_master_screen")
2013-11-12 13:23:19 +0000 Screenshot captured.
```

<h3>Working with masks</h3>
Masks are very important when you use zucchini, because eventually, zucchini will compare you app with different screenshots taken.

If you omit the default one, you will see a failed test with a diff looking like this:
<img src="/images/posts/zucchini/diff.png" />

An easy fix for this is to get a simple mask with a black rectangle where the status bar is located.

```
curl https://raw.github.com/apouche/zucchini-sample/master/features/support/masks/retina_ios7.png \
     -o features/masks/retina_ios7.png
```
