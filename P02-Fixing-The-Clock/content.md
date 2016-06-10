---
title: "Fixing the Clock"
slug: fixing-the-clock
---

We currently have a halfway-functional clock app. The text reads the correct time, but analog clock is stuck at 12:00. Let's fix that!

## Code Organization

In order to figure out what's wrong with the code, let's do a quick tour of some of the files involved. You can follow along by single-clicking on each `.swift` file and glancing at its contents. This code is a bit complex in parts but you'll be able to understand it all by the end of this course!

- __GameViewController.swift__: This loads up a SpriteKit Scene called _FixTheClockScene_, and presents its view as the main view on display. We won't go into much detail here, but you can read about the Model-View-Controller programming pattern at Apple's website, [here](https://developer.apple.com/library/mac/documentation/General/Conceptual/DevPedia-CocoaCore/MVC.html). We'll cover this a bit more later on in the course.

> [info]
> SpriteKit is a 2D game engine. It has been secretly powering all the visualizations from the Playgrounds you worked through earlier!

- __GameScene.swift__: This loads up all of the graphical elements of our Clock scene, using the accompanying SpriteKit Scene file, `FixTheClockScene.sks`. It contains code to update the clock every second, but it retrieves the rotation value for the clock hands from another file...
- __ClockFace.swift__: This file two new data types in it. It uses them to calculate  `Double`s that represent each of the clock hand rotations (in degrees).
  - `NSDate`: stores both date _and_ time
  - `NSCalendar`: helps make sense of the data in `NSDate`

> [info]
> Both `NSDate` and `NSCalendar` are special, complex data types called _class instances_ (sometimes referred to as _objects_). They are created or _initialized_ from special functions called _initializers_.
>
> _Class instances_ or _objects_ can be thought of as collections of values. They can even have functions associated with them. We'll slowly start to work with more and more of these complex data types and even define a few of our own in other exercises!

Did you notice which file was incomplete?

> [solution]
>
```swift
public func getHourHandDegrees() -> Double {
    return 0
}
>
public func getMinuteHandDegrees() -> Double {
    return 0
}
>
public func getSecondHandDegrees() -> Double {
    return 0
}
```

That's right: currently, all the functions in `ClockFace.swift` are returning 0! That can't be right. Let's implement these functions, one at a time.

# NSDate, NSCalendar, and Components

In order to retrieve the hour, minute, and second values of the current time, it is necessary to understand the classes we will be using. The way Swift handles date and time is quite elegant -- especially when you consider how complex it can get due to timezones and alternate (Non-Gregorian) calendars. The concept of a "moment in time" and the "interpretation" of said time, are separated out in its own categories. The `NSDate` class handles retrieval of a "moment in time", and `NSCalendar` handles the "interpretation" of that moment.

If you want more information on these classes, Matt Thompson has an excellent post detailing NSDateComponents [here](http://nshipster.com/nsdatecomponents/).

> [info]
> A _class_ describes all the values and functions that a _class instance_ or _object_ can use. You can think of a _class_ as a blueprint for it's _objects_. Whenever you _initialize_ a new _object_, Swift will use that _class_ as a blueprint to create it.
>
> All these new terms might sound like a foreign language right now and that's okay! They'll become second nature in no time.

## So, how do we actually use NSDate and NSCalendar?

This code is already included in _ClockFace.swift_ but it's a good challenge to try and understand it!

In order to get the current time (in as accurate as milliseconds), you _initialize_ or create a new `NSDate` _object_ from the `NSDate` class. The following code creates a new `NSDate` _object_ and saves it to a variable named `date`.

```swift
var date: NSDate = NSDate()
```

Notice how it looks kind of like a function?

This date object represents a snapshot in time: it contains information about the precise moment in time when you called that _initializer_ (the special function that created a new `NSDate`). In order to get any useful information about that moment in time, though, you need a calendar:

```swift
let calendar: NSCalendar = NSCalendar.currentCalendar()
```

This code looks at your iPhone's time settings, keeping in mind location, locale, 24-hour vs. 12-hour display preferences, and other settings. It returns an _object_ representing how _you_ perceive time in your culture.

For our clock, we want the second, minute, and hour (in 12-hour mode) of the time in our local time zone, so we simply retrieve the information by calling the `component` function on our `calendar` constant. We call functions on _objects_ using dot syntax like below.

```swift
let hour: Int = calendar.component(.Hour, fromDate: date) % 12
let minute: Int = calendar.component(.Minute, fromDate: date)
let second: Int = calendar.component(.Second, fromDate: date)
```

> [info]
> Note that we use the `%`, or _modulo_ operator, to force the hour value to be between the range of 0 and 12. The _modulo_ operator returns the remainder after division of the left number by the right.
>
> - 12 % 12 = 0
> - 7 % 12 = 7
> - 14 % 12 = 2

Now that we know how to retrieve the current time, let's get calculating the degree rotation on each of the hands!
