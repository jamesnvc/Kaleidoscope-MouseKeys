# Kaleidoscope-MouseKeys

![status][st:stable] [![Build Status][travis:image]][travis:status]

 [travis:image]: https://travis-ci.org/keyboardio/Kaleidoscope-MouseKeys.svg?branch=master
 [travis:status]: https://travis-ci.org/keyboardio/Kaleidoscope-MouseKeys

 [st:stable]: https://img.shields.io/badge/stable-✔-black.svg?style=flat&colorA=44cc11&colorB=494e52
 [st:broken]: https://img.shields.io/badge/broken-X-black.svg?style=flat&colorA=e05d44&colorB=494e52
 [st:experimental]: https://img.shields.io/badge/experimental----black.svg?style=flat&colorA=dfb317&colorB=494e52

Have you ever wanted to control the mouse cursor from the comfort of your
keyboard? With this plugin, you can. While it may not replace the mouse in all
situations, there are plenty of cases where one will not have to lift their
hands off the keyboard just to nudge the mouse cursor away a little.

Of course, there is a lot more one can do with the plugin than to nudge the
cursor! Mouse keys are provided for all four directions *and* diagonal movement; mouse
buttons; and a unique warping mechanism. In addition, the speed & acceleration of
the cursor and the speed of the mouse wheel can all be configured to
match one's desired behaviour.

## Using the plugin

To use the plugin, simply include the header, make sure the `MouseKeys` object
is properly `use`'d, and place mouse keys on your keymap. It is best illustrated
with an example:

```c++
#include <Kaleidoscope.h>
#include <Kaleidoscope-MouseKeys.h>

// Somewhere in the keymap:
Key_mouseUp, Key_mouseDn, Key_mouseL, Key_mouseR,
Key_mouseBtnL, Key_mouseBtnR

void setup() {
  Kaleidoscope.use(&MouseKeys);
  
  Kaleidoscope.setup ();
}
```

## Keys provided by the plugin

The plugin provides a number of keys one can put on the keymap, that allow
control of the mouse. They can be divided into a few groups:

### Cursor movement

The simplest set of keys are the mouse cursor movement keys. These move the
cursor in one direction, with a given speed and acceleration. When
a mouse cursor movement key is held down, it will move `.speed` pixels each
`.speedDelay` milliseconds (neglecting acceleration). When `.accelSpeed` is
non-zero (and it is not zero by default), the speed will increase by
`.accelSpeed` every `.accelDelay` milliseconds. Thus, by default, holding 
a direction key will move that way at an increasing speed.

One can hold more than one key down at the same time, and the cursor will move
towards a direction that is the combination of the keys held. For example,
holding the "mouse up" and "mouse right" keys together will move the cursor
diagonally up and right.

The cursor movement keys are as follows:

* `Key_mouseUp`, `Key_mouseDn`, `Key_mouseL`, `Key_mouseR`: Move the cursor up,
  down, left, or right, respectively.
* `Key_mouseUpL`, `Key_mouseUpR`, `Key_mouseDnL`, `Key_mouseDnR`: Move the
  cursor up-left, up-right, down-left, down-right, respectively.

### Scroll wheel

Controlling the scroll wheel is similarly simple. It does not have acceleration,
but one can control the speed with the `.wheelSpeed` and `.wheelDelay`
properties (see below).

* `Key_mouseScrollUp`, `Key_mouseScrollDn`: Scroll the mouse wheel up or down,
  respectively.

### Buttons

Buttons are even simpler than movement: there is no movement speed, nor
acceleration involved. One just presses them.

* `Key_mouseBtnL`, `Key_mouseBtnM`, `Key_mouseBtnR`: The left, middle, and right
  mouse buttons, respectively.

### Warping

Warping is one of the most interesting features of the plugin and is a feature
unique to Kaleidoscope, as far as we can tell. The warping keys position the
mouse cursor within a quadrant of the screen on first press, with any subsequent
taps warping within the previously selected quadrant. For example, pressing
the north-west warp key twice will first jump to the middle of the north-west
quadrant of your screen, then select the north-west quadrant of that, and jump
to the middle of it, resulting in the cursor being three-quarters of the way 
between the center and top-left corner of your screen.

To stop warping, use any other mouse key, or hit the "warp end" key.

This features works out of the box on Windows and OSX. On Linux, a patch exists,
but has not been finalised and merged yet.

The warping keys are the following:

* `Key_mouseWarpNW`, `Key_mouseWarpNE`, `Key_mouseWarpSW`, `Key_mouseWarpSE`:
  Warp towards the north-west, north-east, south-west, or south-east quadrants,
  respectively.
* `Key_mouseWarpEnd`: End the warping sequence, resetting it to the default
  state. Using any of the warping keys after this will start from the whole
  screen again.

## Plugin methods

The plugin provides a `MouseKeys` object, with the following properties available:

### `.speed` and `.speedDelay`

> These two control the speed of the mouse cursor when a movement key is held.
> The former, `.speed`, controls the number of pixels by which the cursor
> moves and defaults to 1. The latter, `.speedDelay`, is the amount of time 
> in milliseconds to wait between two movements and defaults to 0 (no delay).

### `.accelSpeed` and `.accelDelay`

> These two properties control the speed of acceleration. The former,
> `.accelSpeed`, controls by how many pixels the speed shall be increased at
> each step, while the second, `.accelDelay`, controls how often (in
> milliseconds) acceleration should be applied.
>
> They default to 1 pixel and 50 milliseconds, respectively.

### `.wheelSpeed` and `.wheelDelay`

> The last two properties supported by the plugin control the mouse wheel
> scrolling speed. The former, `.wheelSpeed`, controls the number of ticks the
> wheel shall scroll by and defaults to 1. The second, `.wheelDelay`, controls the
> delay between two scroll events and defaults to 50 milliseconds.
