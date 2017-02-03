# tiny_ticks

A very basic timing library for Atmel ATtiny chips.

Basically, you would include the `tiny_ticks.h` header and `tiny_ticks.c` files into your project.
Before including the header, you should ensure that `F_CPU` is set appropriately, otherwise it will default to 8,000,000 (8MHz).

By default the library will enable events, if you do not want the additional code for events included, define `TINY_TICKS_EVENT_ENABLE` as (0) before including the header.

## usage

Once you've included the header, you can get the current "tick" by calling `tinyTicks()`.  This is an inline function and it pulls directly from the volatile variables.  Ideally, you'd set the return value to a variable of type `tick_t` (which defaults to `uint16_t`) and then work with it as opposed to calling the function repeatedly.

In your main loop, you should call `tinyTicksEventLoop()` at each iteration.  This allows the library to keep up with events.  If you don't have events enabled it is mostly a blank callback.  Except you can set the `tinyTicksEventLoopCallback` callback.  This callback should be a `void` function that receives a `tick_t` argument.  The argument will be the number of ticks since the last time the callback was called.  So even if you choose not to do events, you can still make use of the automated calculation of time difference from iteration to iteration.  The callback will only be run if at least one tick has passed since the last iteration.

If you don't disable events, you can register callbacks to run at specific times.  This would be done using the `tinyTicksSetTimeoutInTicks` function, or the `tinyTicksSetTimeout` or `tinyTicksSetTimeoutInMillis` macros.  The first argument is the function pointer.  The function should be a `void` function that takes no arguments.  The second argument is the timeout in ticks, microseconds, or milliseconds; depending on the function/macro you are using.  A tick occurs every 64 clock cycles.

If you want to wait for a specific amount of time, you can use the `tinyTicksDelayInTicks` function, or the `tinyTicksDelay` or `tinyTicksDelayInMillis` macros.  These functions/macros just take the timeout in ticks, microseconds, or milliseconds as the first and only argument.

Technically, you should avoid the delay methods, however they can be useful.  And when coupled with the `tinyTicksEventLoopCallback` you can use the wait time to keep track of other items.  Even when delaying, if events are enabled, they will fire.

## license
The tiny_ticks library is released under the MIT license.

Copyright (C) 2017 Beau Barker (https://github.com/barkerest)
