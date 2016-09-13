---
title: "Degrees Rotation, and Math"
slug: degrees-rotation-math
---

Remember, degrees in a circle go from 0 to 360. In our specific setup, 0 degrees refers to the clock hand in its 12 o'clock position; as the degrees increase, it rotates _clockwise_, until it reaches 12 o'clock at 360 degrees again.

That means that when each second passes, the second hand on a clock moves an amount of 360 degrees divided by 60 seconds, or 6 degrees per second. If you know the second value on your digital clock, you can just multiply that number by 6 to get the rotation in degrees!

Knowing this, let's write the `getSecondHandDegrees()` function. This function gets called every second, just like all the other functions in this class. You can make use of the `date` and `calendar` variables, which are automatically updated by the other code we provided.

> [action]
> Complete the `getSecondHandDegrees()` function. Remember, your function should return a `Double` value, representing the rotation in degrees that the second hand should have. Since `calendar.component(.second, from: date)` returns an `Int` value, you'll have to cast it to a `Double`!

<!--  -->

> [info]
> _Casting_ allows you to change a value from it's original type to another similar type. When we are using division, it's important to avoid using `Int` values -- otherwise we'll end up with only whole numbers!.
>
> You can cast an `Int` value to a `Double` by passing it as a parameter into the `Double()` initializer.
>
> Remember, you can get the current second value with
>
```
let second: Int = calendar.component(.second, from: date)
```

Try filling in the `getSecondHandDegrees` function on your own. Hover over the solution below if you need some help!

> [solution]
> You should have something like this:
>
```swift
func getSecondHandDegrees() -> Double {
    let second: Int = calendar.component(.second, from: date)
    return Double(second) * (360.0 / 60)
}
```

# Minutes and hours

Got it? Now, let's do the same for the minute and hour hands, using the same concept. Once you're done, run your code and see how it compares to an actual analog clock. Is your math correct?

> [action]
> Complete the two remaining functions – `getHourHandDegrees()` and `getMinuteHandDegrees()`. Do the same thing that you did with seconds, except use the `.minute` component now!

<!--  -->

> [info]
> Remember, you can get the current hour, minute, second values with
>
```swift
let hour: Int = calendar.component(.hour, from: date) % 12
let minute: Int = calendar.component(.minute, from: date)
let second: Int = calendar.component(.second, from: date)
```

<!--  -->

> [solution]
> Compare your code with the solution here. If yours looks a little more complicated than the code shown here, hang tight and don't change your code – you might be ahead of the game!
>
```swift
func getHourHandDegrees() -> Double {
    let hour: Int = calendar.component(.hour, from: date) % 12
    return Double(hour) * (360.0 / 12)
}
>
func getMinuteHandDegrees() -> Double {
    let minute: Int = calendar.component(.minute, from: date)
    return Double(minute) * (360.0 / 60)
}
```

# That doesn't quite look right...

You might've noticed that if you look at an analog clock pointing at 6:30 for instance, the hour hand doesn't point exactly at 6. When it's _halfway_ to 7 o'clock, the hour hand is similarly _halfway_ between 6 and 7. Our current code doesn't actually do this. Let's fix that.

## Fixing the hour hand

Since there are 12 hours in a 360-degree hour hand rotation, the hour hand moves 30 degrees (360 / 12) each hour. For each minute within an hour, the hour hand should move 1/60th of that space. That means that for each minute, the hour hand moves 0.5 degrees (360.0 / 12 / 60). Let's write that out in code.

```swift
func getHourHandDegrees() -> Double {
    let hour = calendar.component(.hour, from: date) % 12
    let minute = calendar.component(.minute, from: date)
    return Double(hour) * (360.0 / 12) + Double(minute) * (360.0 / 12 / 60)
}
```

But wait! What about the seconds? Doesn't the hour hand move a minuscule amount for each second, too?

You're right. Can you figure out the complete equation for the hour hand degree movement?

> [action]
> Complete the `getHourHandDegrees()` function, using all of the hour, minute, and second values into account when calculating the rotation in degrees.

<!--  -->

> [solution]
> Your code should look like this:
>
```swift
func getHourHandDegrees() -> Double {
    let hour = calendar.component(.hour, from: date) % 12 // just in case it returns 24-hour time
    let minute: Int = calendar.component(.minute, from: date)
    let second: Int = calendar.component(.second, from: date)
    return Double(hour) * (360.0 / 12) +
           Double(minute) * (360.0 / 12 / 60) +
           Double(second) * (360.0 / 12 / 60 / 60)
}
```

## Fixing the minute hand

Good job! Now can you do the same for the minute hand?

> [action]
> Grab some pen and paper if you haven't already and figure out the math. After you have an equation, put it into code! Don't forget to cast your `Int`s into `Double`s!

<!--  -->

> [solution]
> Your code should look like this:
>
```swift
func getMinuteHandDegrees() -> Double {
    let minute: Int = calendar.component(.minute, from: date)
    let second: Int = calendar.component(.second, from: date)
    return Double(minute) * (360.0 / 60) +
           Double(second) * (360.0 / 60 / 60)
}
```

Run your code now. Congratulations – that looks just like a true analog clock!

In the next section, you'll learn how to transfer this project to your iPhone! If you don't have an iOS device on you, you can skip this next section.
