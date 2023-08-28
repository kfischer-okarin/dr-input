# dr-input

A simple input control for DragonRuby.

## Usage

### Simple

```ruby
require 'lib/input.rb'

def tick(args)
  # Create an input
  args.state.input ||= Input.new(x: 100, y: 600, w: 300)

  # Allow the input to process inputs and render text (render_target)
  args.state.input.tick

  # Get the value
  args.state.input_value = args.state.input.value

  # Output the input
  args.outputs.primitives << args.state.input
end
```

### Constructor Arguments

* `x` - x location, default 0
* `y` - y location, default 0
* `w` - width, default 256
* `h` - height, default is the height of the font (as measured by `calcstringbox`) + 2 * the `padding`
* `value` - initial input value (string), default ''
* `padding` - padding, default 2
* `font` - font path (eg. `'fonts/myfont.ttf'`), default ''
* `size_enum` - size enumeration (integer), or named size such as `:small`, `:normal`, `:large`, default: `:normal` (`0`)
* `r` - font color, red component, default 0
* `g` - font color, green component, default 0
* `b` - font color, blue component, default 0
* `a` - font color, alpha component, default 255
* `background_color` - background color, can be array or hash format, default nil
* `word_chars` - characters considered to be parts of a word, default `('a'..'z').to_a + ('A'..'Z').to_a + ('0'..'9').to_a + ['_', '-']`
* `punctuation_chars` - charcters considered to be punctuation, default `%w[! % , . ; : ' " \` ) \] } * &]`
* `selection_start` - start of the selection (if any) in characters (Integer), default the length of the initial value
* `selection_end` - end of the selection (if any) and the location of the cursor in characters (Integer), default `selection_start`
* `selection_r` - selection color, red component, default 102
* `selection_g` - selection color, green component, default 178
* `selection_b` - selection color, blue component, default 255
* `selection_a` - selection color, alpha component, default 128
* `key_repeat_delay` - delay before function key combinations (cursor, cut/copy/paste and so on) begin to repeat in ticks (Integer), default 20
* `key_repeat_debounce` - number of ticks (Integer) between function key repeat, default 5
* `word_wrap` - if the control should wrap (Boolean), default false
* `focussed` - initial input focus (Boolean), default false
* `on_clicked` - on click callback, receives 2 parameters, the click and the `Input` control instance, default NOOP
* `on_unhandled_key` - on unhandle key pressed callback, receives 2 parameters, the key and the `Input` control instance, default NOOP. This callback receives keys like `[tab]` and `[enter]`

### Instance Methods

* `#insert(text)` - Inserts text at the cursor location, or replaces if there's a selection
* `#focus!` - Focusses the instance. Note the instance will only receive the focus after it's rendered. This prevents multiple instances from handling the keyboard and mouse events in the same tick.
* `#blur!` - Removes the focus from the instance. This happens immediately and the instance will not process keyboard and some mouse events after being blurred.


## Notes

* Adding a `background_color` significantly improves the rendering of the text.

## Thanks

* @danhealy for Zif. The Zif Input was the starting point for this. Though you wouldn't be able to tell now, it was a really solid place to start.
* @leviondiscord (on Discord, aka @leviongithub) for suggesting `#delete_prefix` when I would have done something much dumber. And also providing other interesting methods I'm likely to use at some point
* @DarkGriffin (on Discord) for requesting this control in the first place, and not being shy about the _crazy_ desired feature list (of which, I feel like, I've only touched the surface).
* @aquillo (on Discord) for asking me (and others) to review his code, where I learnt that the value returned by `keyboard.key` is the `tick_count` the key was pressed which made implementing key repeat much simpler than the silly thing I would've done.
