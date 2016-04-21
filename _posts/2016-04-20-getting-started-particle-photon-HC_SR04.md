---
layout: post
title: Getting started with Particle Photon and HC-SR04 distance sensor
---

If I were to setup Particle Photon and HC_SR04 as a brand new user I wish someone has given me some points on how to go about doing it so I would not be perplexed and spend too much time deciphering. Here you go. I hope you find it useful. ([I found this useful](https://community.particle.io/t/simple-photon-ping-sensor-hc-sr04/16737))

Things I found with WiFi and [Particle Photon](https://store.particle.io/collections/photon) that you may find useful.

- Initial setup ([The Claiming](https://docs.particle.io/guide/getting-started/start/photon/#connect-your-photon))
  - DO setup photon on your own home WiFi.
  - DO NOT try to connect Photon to [WPA2 Enterprise](https://community.particle.io/t/particle-in-the-enterprise/14085) networks. Not supported.
  - DO NOT use public wifi hotspots that present [Captive Portal](https://en.wikipedia.org/wiki/Captive_portal).

- Connectivity issues after initial setup:
  - DO set internal only antenna if you have issues with Photon dropping connection. (default is Auto select and it may be causing that problem. NOT scientifically verified [but I saw a note here](https://community.particle.io/t/photon-wifi-range/16735))
  - If you bring Photon to show off to your friend and you are not sure about their WiFi then before coming over setup your phone tethering on (with WPA2 PSK or None) and add that WiFi setup to your Photon. The drawback to this is if you want to use Particle app on your phone to setup your Photon but you can impress your friends if you want to run a nodebot or show them how your photon communicates with the cloud

## Build a Thing for Internet of Things

I am assuming you have already claimed your Photon using either phone app or USB setup. If that's all you have done you may want to play with hooking up some sensor (You probably did a turn on/off an LED already)

The next step is to get a breadboard and some sensor device examine the Internet Of Things and the whole Cloud tools that Photon is providing.

The following uses HC_SR04 range sensor. [Google it, get it cheap on eBay](https://www.google.com/search?q=HC_SR04+buy)

- Hardware: [Particle Photon](https://www.particle.io/). (and a small breadboard) with USB cable for power from Macbook (I use Macbook but you can use Raspberry PI maybe). (BEWARE: some USB cables are only for power. You want USB cable for data)
  - Firmware is at 0.4.9 (as seen on Web IDE)
- Hardware: (I use Nexus 5 phone). Software: Particle Phone app. (can't really use it when my phone is in hotspot mode)
  - Phone has a built in Tinker app that you can use to write and read D and A pins on the board. That is neat
- Hardware: Macbook or some keyboard aware machine
  - Software: Install 'particle-cli' nodejs module. (when claiming your photon beware of WiFi config issues)
  - Web IDE https://build.particle.io/build/ This is nice.
  - Software: Particle Dev. (Even if you have it locally the code still goes up to the Web IDE)
    - I have not found any use for it yet. It's build on top of Atom. I do use Atom daily.
    - Your local macbook IDE https://www.particle.io/dev based on Atom.
    - has compile capability:  https://docs.particle.io/guide/tools-and-features/dev/#compiling-code (always make new folder for a project) (compiles in to a .bin file) but really I would stick with Web IDE for now.
    - Has serial monitor but I have not used it. (Instead I use `particle-cli` for all that)
    - cloud variables and functions: Awesome idea. Expose variables and functions to be queried and triggered with REST API.

And finally a HC_SR04 sensor to connect to Photon:

<blockquote class="twitter-tweet" data-lang="en"><p lang="en" dir="ltr">Getting HC_SR04 working on particle photon. I want to add distance sensor to nodebot car <a href="https://t.co/2kDHySNvt3">pic.twitter.com/2kDHySNvt3</a></p>&mdash; яuвyтеsтеr (@rubytester) <a href="https://twitter.com/rubytester/status/719317099567407109">April 11, 2016</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

Things to do with hardware:

Let's play with HC_SR04 sonar distance measuring sensor on a breadboard. Connect 4 wires. 2 wires for power/ground. A wire for Echo pin connect to D5 pin on Photon, and Trigger pin connect to D4 Photon pin.

Things to do with software:

- Open [Web build IDE](https://build.particle.io/build/new). We'll build new file to load on your photon
- Select your device. (devices are under 'target scope' icon). Mouse over your photon name, see a star appear to the left of the name? click it, it turns yellow. Now you can download code to your device (you flash it)). I think that icons shoud be really a red target scope rather than a yellow star. You are selecting a target and not making a favorite.
- Click on Code (like github icon "< >" for code) and create new app called "distance".
- Click on Libraries (the bookmark icon). In 'Community Libraries' section type 'HC_SR04' and select it
  - Now a choice, what do I do? Do I 'Use This Example'? 'Fork'? or 'Include in App'?
  - let's 'Include in App' cause you are building a new app
    - but before you include BEWARE: you can't just type the following source text at the top of your app file you just created `#include "HC_SR04/HC_SR04.h"`
     you actually have to click that 'Include In App' button which actually does the job of including. You are asked at this point 'Which app?' The UI may be a bit nicer at this point and ask you if you want new app created.
    - Click to select your 'DISTANCE' app name (yes, it's all upcased) and 'ADD TO THIS APP' and you will now see you have your app and your included libraries has HC_SR04

Now you 'DISTANCE' app should display a `distance.ino` file with contents:

```arduino
// This #include statement was automatically added by the Particle IDE.
#include "HC_SR04/HC_SR04.h"

void setup() {

}

void loop() {

}
```

OK, let's replace it with this code: (notice we are leaving the #include that was added there)


```arduino
// This #include statement was automatically added by the Particle IDE.
#include "HC_SR04/HC_SR04.h"

int cm = 0;
int trigPin = D4;
int echoPin = D5;

HC_SR04 rangeFinder = HC_SR04(trigPin, echoPin);

void setup()
{

}

void loop()
{
    cm = rangeFinder.getDistanceCM();
    Particle.publish("distance", (String) cm);
    delay(10000);
}
```

This would be a good time to patiently [read the example code presented in 'HC_SR04' library.](https://github.com/simonmonk/Spark_HC_SR04/blob/master/firmware/examples/HC_SR04_Variables.cpp)

I made minor changes to simlify and make it show up in the cloud. We gonna publish the readings every 10 seconds to the cloud. The original code exposes the `cm` and `inches` variables but you have to query the device to get their values. Oh no no my dear friend, we be cloud publishing now.

OK, save the app, Verify that it compiles. It does? Good. Flash it and [open the dashboard logs page to watch some action](https://dashboard.particle.io/user/logs)


Your photon should get this code and start sending sensor data to the cloud visible in the logs page

Move stuff close and away from the sensor and watch it update the logs every 10 seconds.

All the best and keep exploring.

