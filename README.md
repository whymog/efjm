![Escape from Juggalo Mountain title](src/img/title_screen_image.png "Logo Title Text")

# Escape from Juggalo Mountain: Act I

A visual novel built with [Twine](http://twinery.org/) about coming of age in a separatist society. With face paint.

# Setup

_Escape from Juggalo Mountain: Act I_ requires [twee2](https://github.com/Dan-Q/twee2), an open-source, command-line tool for compiling [Twine](http://twinery.org/) games. However, as of this writing, the `twee2` ruby gem is outdated, so the game won't compile to Harlowe2 (our preferred Twine story format) correctly.

To get around this, you'll need to clone the latest version of `twee2` and install it manually, using these instructions (adapted from this helpful [comment](https://github.com/Dan-Q/twee2/issues/36#issuecomment-383277996)):

```
git clone https://github.com/Dan-Q/twee2
cd twee2/
gem build twee2.gemspec
gem install twee2-0.5.0.gem
```

_February 2023 Update_: The version of ruby specified in twee2's `.ruby-version` file, 2.1.5, cannot be built or run successfully on modern Mac OSes (Intel or otherwise). Here's what worked for me:

1. Install [`rbenv`](https://github.com/rbenv/rbenv)
2. In twee2's `twee2.gemspec` file, change the following line:  
   `spec.add_runtime_dependency 'bundler', '~> 1.6'`  
   to:  
   `spec.add_runtime_dependency 'bundler', '~> 2.1.4'`
3. Use ruby 2.7.7 to build the twee2 gem (`rbenv local 2.7.7` to set the local version)
4. Copy the gemfile into the root `/efjm` directory
5. Using the same 2.7.7 version of ruby, run `gem install twee2-0.5.0.gem`

To confirm this worked, try running `twee2 formats`. If it installed correctly, you should see `Harlowe2` in the list of results.

# Developing

## Background

If you use a plaintext editor like Sublime Text or Atom, there are `twee2` syntax highlighting plugins you can find. Syntax highlighters make it easier to understand the logic of the code you're writing at a glance. I've been using [this Atom plugin](https://github.com/bvautour/language-twee2) and it's been great.

Most of the work you'll be doing is in the `src/` directory. Each of the game's "chapters" is broken up into a discrete `.tw2` file, which is imported into the main `efjm.tw2` file via the special `::StoryIncludes` macro. You can learn more about how that works [here](https://dan-q.github.io/twee2/documentation.html#includes). If you're going to add or rename a section, make sure you update its import statement in the `::StoryIncludes` section; otherwise, your builds will error out.

## Building

To build the game, from the root directory of your project, you'll want to use `twee2 build`. This command takes several parameters:

`twee2 build [input.tw2] [output.html] [[--format=story_format]]`

I recommend using the following command:

`twee2 build src/efjm.tw2 build/efjm.html --format=harlowe2`

Or if you'd like `twee2` to keep an eye out for changes as you make them, you can set it to automatically generate a new build of your game every time you save a file with:

`twee2 watch src/efjm.tw2 build/efjm.html --format=harlowe2`

(Note: This process runs perpetually until you press `^C`, so you may wanna run this in a new terminal window or tab.)

This instructs `twee2` to build the game from the `src/efjm.tw2` file (which imports all other game modules) and outputs it to a separate `build/` directory, which is git-ignored. It also ensures the game is built for the Harlowe2 story format.

If you're unable to get `twee2` to build to Harlowe2 for any reason, you can export to whatever story format you like. However, you'll need to import your HTML-output game into Twine, open it up in the editor, and change the story format there to Harlowe2 and rebuild it before the game will work properly. This is a bummer, since it means you won't be able to use `twee2 watch` to automatically rebuild your game as you're working on it, but life's full of little compromises.

# Credits

A game by Nick Cummings, Ben Morgan, and Nathan Harrison

Illustrations by Sarah Morgan

Map of Juggalo Mountain and surrounding territories by Nick Cummings

Playtesting by Yelena Kolova and Dan Phipps

Special thanks to:

- Sean Dunne
- Matt Gilles
- Ian McAfee
- Nathan Rabin
