---
title: "Making A BLDC Motor Go Brr"
date: 2020-11-05T04:29:53-04:00
slug: "motor_go_brr"
description: "this took me 4 weeks why is this so hard"
keywords: ["brushless motors", "odrive", "flask", "socketio", "websocket", "control"]
draft: false
tags: ["robotics", "web dev", "swol kat"]
math: false
---

## Background 

A little bit of background on this project, this post is about my MQP(Major Qualifying Project) it's basically my schools version of a thesis. For my project, I decided to a thing my parents never let me do. Get a dog. Kinda ... 

In reality my team and I are working on a quadruped. Other schools/companies have produced some amazing work as seen in this video.

<br/>

{{< youtube 5VVkkvjxF_c >}}


But our school, while having attempted the project multiple times, hasn't really made a quadruped that works. 

So the singular goal of this project is to fix that. Our goal is to make a quadruped that can preform a dynamic walking gait. 

## The Actuators. 

### What we need

So the core of this project are the actuators, finding actuators that can do what we need is actually surprisingly difficult, we need high torque, low rpm actuators and for those of you that aren't robotics students, this is not common place and colleges and students trying to design robust actuators has hot spot in research for ages. Now traditional engineering approach says for us to build a quadruped we must first do all the math about it and use that math to determine the ideal actuator and control system needed. However there's a few problems with that. 

1. I have no idea how to do that math. Dynamics of quadrupeds is a hard problem and being honest with you as some one that has never had to touch anything over basic dynamics in my undergraduate education it would be foolish to hold up the entire project while my team attempts to figure out how to do the math (Though we absolutely will be doing that math at some point.) 

2. Spending all this time on trying to design or make this perfect actuator is a distraction from the real point of the project, our goal is to make a quadruped walk not design a beautiful robotics actuator. Other teams that have pulled off this project have had 3-4 years worth of iterations and testing to get something to a workable state. We don't have that time, so rather than trying to reinvent the wheel, we are trying to piggy back off as much previous work as possible. 

### What we are using

So now the question is, what is an motor, controller combo that we know will work? Reading through the papers on MIT's [Mini Cheetah](https://build-its.blogspot.com/2019/12/the-mini-cheetah-robot.html) and Stanford's [Doggo](https://github.com/Nate711/StanfordDoggoProject), we settled on using the [Turningy Multistar 9225 160kv BLDC](https://hobbyking.com/en_us/9225-160kv-turnigy-multistar-brushless-multi-rotor-motor.html?queryID=&objectID=40516&indexName=hbk_live_magento_en_us_products) they were right in the range of what previous projects had used and even better my roommate had 8 of them just lying and kindly allowed us to use them for this project. To pair with this motor we went with the [Odrive Controller](https://odriverobotics.com/) previous projects have had good success with this although (as you'll read later) I struggled for a few weeks to get it to behave as I expected. 


### How to make a motor go brr. 

Okay so according to the odrive documentation setting up the motor with odrive *should* be pretty simple and getting a small encoder working with the optical encoder too