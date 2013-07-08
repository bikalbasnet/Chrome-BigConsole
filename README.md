Adds a panel to the Chrome Developer Tools that provides a multi-line split
console, like in Firebug, with syntax highlighting.

![Screenshot](https://raw.github.com/IceCreamYou/Chrome-BigConsole/master/screenshot.png)

## Why?

Firefox is my primary browser, I like the split console in Firebug, and I hate
that Chrome doesn't have that, so I fixed it.

Sometimes you just want a block of code that persists beyond execution.
Especially when you're running a lot of code in the console, you don't want to
hit `shift+enter` to get a newline. Also, syntax highlighting.

You could also just use [FirebugLite](https://getfirebug.com/firebuglite), but
it is not persistent and appends itself to the DOM. There are also a lot of
things it can't do since it is part of the page and not the chrome, so that
means you have to switch back and forth with devtools. BigConsole is easier.
Also, syntax highlighting.

## Implementation

BigConsole is currently implemented as a new panel in the Chrome developer
tools. It would be better if it could override the default Console tab but
Chrome does not currently expose the APIs to do that.

There are some limits to the way code is currently being evaluated. Most
importantly, the result of evaluation is converted to JSON and back before this
extension receives it, which means only acyclic objects can be inspected and
object types are not preserved. I have an idea for how to get around this but
it is pretty complicated and maybe not worth the effort.

**Note:** You can click on truncated console output to expand and collapse it.
Also, the keyboard shortcut `CTRL+Enter` (or `Command+Enter` for Macs) will run
the code in the editor as if you pressed the "Run" button.

## Status

BigConsole currently does most of what it was designed to do. That is, it
provides a multiline console and log which evaluates JavaScript in the page
context just like the default devtools console. However, there are a bunch of
features / fixes that would make it much better:

- Allow printing cyclic objects to the console
- Add executed command history
- Output into the log should get pretty-printed better and ideally it should be
  possible to inspect returned objects. (This will probably be done by including
  another library...)
- `console.log` currently only works as expected when it is called
  synchronously. If `console.log` is called asynchronously after the code
  finishes evaluating then it will actually try to record to the default
  Console panel because the `console` object is swapped out only temporarily
  while BigConsole scripts run.
- No `console` commands other than `console.log` are supported yet.
- It would be nice if commands that went to the default Console panel also went
  to the BigConsole, and vice versa. There are some experimental APIs that will
  make this possible, so once those hit the stable branch this could be
  worth investigating.
- The extension needs an icon for the extensions page!

## Installation

1. Clone the code
2. Go to `chrome://extensions`
3. Check the "Developer mode" checkbox at the top of the page
4. Click the "Load unpacked extension..." button which will appear at the top
   of the page
5. Navigate to the folder with the extension code in it and click "OK"

Then go to any page you want to inspect, open the devtools (CTRL+SHIFT+I), and
switch to the BigConsole tab.

To upgrade, just update the code (e.g. with `git pull`), then go to
`chrome://extensions` and click the "Reload (CTRL+R)" link under the extension.
If you have the devtools open, you'll have to close and reopen it before the
upgraded version will be loaded.

## Credits

[Isaac Sukin](http://www.isaacsukin.com/contact)
([@IceCreamYou](https://twitter.com/IceCreamYou)) is the author of this project.

Contributions are very much welcome!

The code is released under the [MIT License](http://opensource.org/licenses/MIT)
except the ace editor code, which is released under the BSD license as described
at the top of the relevant files.
