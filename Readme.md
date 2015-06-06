Cursive
=======

Cursive is a ncurses-based TUI (Text User Interface) library for rust. It is based on jeaye's [ncurses-rs](https://github.com/jeaye/ncurses-rs).

It is designed to be safe and easy to use:

```
[dependencies.cursive]
git = "https://github.com/Gyscos/cursive"
```

(You will also need ncurses installed - if it isn't already, check in your package manager.)

```rust
extern crate cursive;

use cursive::Cursive;
use cursive::view::{Dialog,TextView};

fn main() {
	// Creates the cursive root - required for every application.
    let mut siv = Cursive::new();

    // Create a popup window with a button that quits the application
    siv.add_layer(Dialog::new(TextView::new("Hello Dialog!"))
                    .title("Cursive")
                    .button("Quit", |s| s.quit()));

    // Starts the event loop.
    siv.run();
}
```

![Cursive dialog example](https://raw.githubusercontent.com/Gyscos/Cursive/master/doc/cursive_example.png)

_(Colors may depend on your terminal configuration.)_


The goal is to be flexible enough, so that recreating these kind of tools would be - relatively - easy (at least on the layout front):

* [menuconfig](http://en.wikipedia.org/wiki/Menuconfig#/media/File:Linux_x86_3.10.0-rc2_Kernel_Configuration.png)
* [nmtui](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Networking_Guide/sec-Configure_a_Network_Team_Using_the_Text_User_Interface_nmtui.html)

A few notes :

* The main focus point is _not_ performance. This is a simple layout library, guys, not [compiz](https://www.google.com/search?q=compiz&tbm=isch) piped into [libcaca](https://www.google.com/search?q=libcaca&tbm=isch). Unless you are running it on your microwave's microcontroller, it's not going to be slow.
* The library is single-threaded. Thus, callback methods are blocking - careful what you're doing in there! Feel free to use threads on your side, though.
* This goal is _not_ to have an equivalent to every ncurses function. You _can_ access the underlying ncurses window when creating your own custom views, so you can do what you want with that, but the main library will probably only use a subset of the ncurses features.

Compatibility
-------------

First off, terminals are messy. A small set of features is standard, but beyond that, almost every terminal has its own implementation.

I mostly test VTE-based terminals (Gnome & Xfce), with the occasional Konsole and xterm checks.

### Output

* **Colors**: the basic 8-colors palette should be broadly supported. User-defined colors is not supported in the raw linux TTY, but should work in most terminals, although it's still kinda experimental.
* **UTF-8**: Currently Cursive really expects a UTF-8 locale. It may eventually get patched to support window borders on other locales, but it's not a priority.
Also, Cursive currently expects every codepoint to be a one-column character, so some things may break with exotic characters...

### Input

The `key_codes` example can be a useful tool to see how the library reacts to various key presses.
* **Basic set**: All simple key press (without shift, alt or ctrl pressed) should work in all terminals.
* **Shift+keypress**: All characters keys (letters, numbers and punctuation) should work as expected.
* **UTF-8**: UTF-8 input should work fine in a unicode-enabled terminal emulator, but raw linux TTY may be more capricious.
* **Control codes**: 

Contribute
----------

You want to help? Great! Here is a non-exhaustive list of things you could do:

* Provide example use-case: a good idea of application for existing or new components.
* Test and reports issues: a bug won't get fixed if we don't know it's there.
* Hack the code! If you feel confident with rust, pick an issue you like and hack away!
