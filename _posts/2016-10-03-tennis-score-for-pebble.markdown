---
title:  "Tennis Score for Pebble"
date:   2016-10-03 15:04:23
categories: [smart watch, software development]
tags: [pebble, C, tennis]
---
Do you play tennis and own a [Pebble][pebble] watch? Now you can keep track of the score in your Pebble! <br>
[**Tennis Score**][pebbleapp] for [Pebble][pebble] tracks points, games, sets and serves. Update the score by pressing a button. Choose between best of 3 or 5 sets; tiebreak for all sets. Shows both current time and match duration.
<br>[**Tennis Score**][pebbleapp] for [Pebble][pebble] works in Pebble Time Round, Time Steel, Time, Steel and Classic.

## Usage
**Menu**

- Select between best of 3 or 5 sets

- Select who serves first

- Start match!

**Match**
Use [buttons](http://i.imgur.com/4i9NeDU.jpg) to track the score:

- Up: point to your opponent

- Down: point to you

- Select: undo last point; as many times as you want

- Back: exit to main menu

**Score**

- Upper row: your opponent's score

- Lower row: your score

- Left time: current time

- Right time: match duration

- Asterisk: who is serving

**Statistics**
Scroll down at the end of the match:

- Match score

- Number of points won

- Match duration

- Set duration

## Screenshots

![](../../images/pebble-time-round-black-menu.png)

![](../../images/pebble-time-red-menu.png)

![](../../images/pebble-time-round-red-score.png)

![](../../images/pebble-time-black_score.png)

![](../../images/pebble-orange-score.png)

![](../../images/pebble-time-round-black-sets.png)

![](../../images/pebble-time-red-points-won.png)

![](../../images/pebble-time-red-match-duration.png)

![](../../images/pebble-time-round-red-sets-duration.png)

![](../../images/pebble-time-round-red-sets-duration-2.png)


## About
Code is [here](https://github.com/gborobio73/tennis-score-c).

Dialog and message windows are based on Pebble examples [UI Patterns](https://github.com/pebble-examples/ui-patterns):

Limitations: [**Tennis Score**][pebbleapp] for [Pebble][pebble] keeps track up to 180 points for Pebble Classic and Steel, and up to 600 points for other models. Statistics are not available for Pebble Classic nor Steel.

[**Tennis Score**][pebbleapp] for [Pebble][pebble] is my first ever app for Pebble and it's written in C.

[pebbleapp]: https://apps.getpebble.com/applications/57b1c129bb85ed22da0004ce
[pebble]: https://www.pebble.com/
