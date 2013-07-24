---
categories: sldcode
layout: post
title: Smooth zoom animation in HTML5
---

I wanted to insert some nice-looking animation into my HTML5 project when zooming and scrolling. I'm not a math expert, so here's my stab at it.

I decided I'll try an "expected result over time function": I wanted to have something that starts up gradually and slows down when approaching the end of the allowed timeframe.

- Find a function that in the range of `[0, f]` will have the result of `[0, 1]`.
- Steepness and duration should be part of the function.
- It must be a sigmoid function.

Wolfram Alpha is good resource to start looking when you're not exactly a math expert (I am not...). I ended up with [solve coth(s)\*tanh((s/f)n) = 1 where s = 4 and f = 120](http://www.wolframalpha.com/input/?i=solve+coth%28s%29%2Atanh%28%28s%2Ff%29n%29+%3D+1+where+s+%3D+4+and+f+%3D+120) as a working solution.

- *s* is the steepness factor;
- *f* is the total number of frames for the animation;
- *n* is the frames elapsed since the start of the animation.

The (draft of the) generalized diff function is:

    var ValueDiffFunction = function(start_value, target_value, start_frame, target_frame) {
      this.start_value = start_value;
      this.target_value = target_value;
      this.start_frame = start_frame;
      this.target_frame = target_frame;
    };
    ValueDiffFunction.prototype.next_value = function(value, frame) {
      if (this.target_value == null) {
        return value;
      }
      var result = value;
      if (frame >= this.target_frame) {
        result = this.target_value;
      } else {
        var total_value_diff = this.target_value - this.start_value;
        var total_frames = this.target_frame - this.start_frame;
        var n = frame - this.start_frame;

        var steepness = 4;
        var functionresult = coth(steepness) * tanh((steepness / total_frames) * n);
        result = this.start_value + total_value_diff * functionresult;
      }
      return result;
    };
