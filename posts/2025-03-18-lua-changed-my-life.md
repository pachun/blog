# Lua Changed My Life

_March 18th, 2025_

[Lua](2) changed my life.

I love [Ruby](3) because it's the nicest programming language I've ever worked with. I'm using [Elixir](4) because I'm into functional programming concepts. I've been using [React](6) for a decade because it made front end development tolerable. I like making iPhone apps, so I'm a big fan of [Expo](9) & [React Native](7).

I don't like python.

Regardless of which programming language I'm using, there are several tools that I've used (almost) every day for well over a decade (~13 years): [Neovim](13), [Git](11) and [Tmux](12).

About a year ago, I learned that Lua had become Neovim's default configuration language. [Since then, my dotfiles have evolved to place that I'm actually proud of](14).

Before Lua, I floundered to configure Vim with VimScript. It felt arcane. I disappointed [friends and colleagues](https://github.com/blakewilliams) by abusing tools they love. I misused tmux panes to (in part) compensate for my lack of vim configuration for over a decade. I had been opening a vim instance in each tmux pane to see all the different relevant bits of a bunch of files at once. After Lua, I stopped using doing that. I configured vim panes to work the way I'd been abusing tmux panes (hotkey-wise), to ease the transition and take advantage of vim, because Lua made it easy to configure.

I quickly stopped using the tmux-to-vim-pane crutch that I created because ... Lua made it easy for me to install an LSP. Jumping directly to the definition of somthing, and then right back out to where you had been is _so_ much better than opening a tmux pane and new vim instance. I stopped typing `vim full/file/path.rb` because Lua made it approachable for me to install and configure telescope.

After learning about Lua, it only took a couple months for me to start using all the tools I'd been using prior, but with the confidence to configure Neovim, which led to me using the others for their intended purposes. Which has been awesome:

<img src="/assets/2025-03-18-lua-changed-my-life/tmux.png" alt="tmux" style="max-width: 200px;">

Lua enabled me to start using vim correctly, customizing it wherever I felt appropriate. Which encouraged me to start thinking about how my other tools _should_ be used and then start using them that way.

Lua is better than Python.

[All Posts](/README.md)

[2]: https://www.lua.org
[3]: https://www.ruby-lang.org
[4]: https://elixir-lang.org
[6]: https://reactjs.org
[7]: https://reactnative.dev
[9]: https://expo.io
[11]: https://git-scm.com
[12]: https://github.com/tmux/tmux
[13]: https://neovim.io
[14]: https://github.com/pachun/boo
