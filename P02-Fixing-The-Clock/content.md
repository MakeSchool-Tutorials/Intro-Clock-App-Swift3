---
title: "Fixing the Clock"
slug: fixing-the-clock
---

Where we left off, we were looking at a halfway-functional clock app: The text read the correct time, but analog clock was stuck at 12:00. In this section, we'll fix that!

## Code Organization

In order to figure out what's wrong with the code, let's do a quick tour of some of the classes involved. You can follow along by selecting each `.swift` file and glancing at its contents.

- __GameViewController.swift__: This loads up a SpriteKit Scene called "FixTheClockScene", and presents its view as the main view on display. We won't go into much detail here, but you can read about the Model-View-Controller programming pattern at Apple's website, [here](https://developer.apple.com/library/mac/documentation/General/Conceptual/DevPedia-CocoaCore/MVC.html).
- __GameScene.swift__: This loads up all of the graphical elements of our Clock scene, using the accompanying SpriteKit file, `FixTheClockScene.sks`. It contains update code that fires a clock update function, but it retrieves the value of how much to rotate each of the hands from another class...
- __ClockFace.swift__: This class has some basic utility code that when given a `NSDate` (Apple's way of storing information both date _and_ time), and a `NSCalendar` (A class that handles the calendar system in which you _interpret_ the current date and time), it will return `Double`s that represent each of the clock hand rotations, in degrees.

Can you see which one of these files are incomplete?

![Incomplete ClockFace class](./assets/clockface-incomplete.png)

That's right: currently, all the functions in `ClockFace.swift` are returning 0! That can't be right. Let's implement these functions, one at a time.

# NSDate, NSCalendar, and Components
