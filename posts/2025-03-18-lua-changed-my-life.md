# Lua Changed My Life

_March 18th, 2025_

Lua changed my life.

I love Ruby because it's the nicest programming language I've ever worked with. I'm using Elixir because I'm into functional programming concepts. I've been using React for a decade because it made front end development tolerable. I like making iPhone apps, so I'm a big fan of Expo & React Native.

I don't like python.

Regardless of which programming language I'm using, there are several tools that I've used (almost) every day for well over a decade (~13 years): Neovim, Git and Tmux.

About a year ago, I learned that Lua had become Neovim's default configuration language. [Since then, my dotfiles have evolved to place that I'm actually proud of][1].

Before Lua, I floundered to configure Vim with VimScript. It felt arcane. I disappointed [friends and colleagues][2] by abusing tools they love. I misused tmux panes to (in part) compensate for my lack of vim configuration for over a decade. I had been opening a vim instance in each tmux pane to see all the different relevant bits of a bunch of files at once. After Lua, I stopped using doing that. I configured vim panes to work the way I'd been abusing tmux panes (hotkey-wise), to ease the transition and take advantage of vim, because Lua made it easy to configure.

I quickly stopped using the tmux-to-vim-pane crutch that I created because ... Lua made it easy for me to install an LSP. Jumping directly to the definition of somthing, and then right back out to where you had been is _so_ much better than opening a tmux pane and new vim instance. I stopped typing `vim full/file/path.rb` because Lua made it approachable for me to install and configure telescope.

After learning about Lua, it only took a couple months for me to start using all the tools I'd been using prior, but with the confidence to configure Neovim, which led to me using the others for their intended purposes. Which has been awesome:

<p align="center">
  <img src="/posts/assets/2025-03-18-lua-changed-my-life/tmux.png" width="600" alt="tmux" />
</p>

Lua enabled me to start using vim correctly, customizing it wherever I felt appropriate. Which encouraged me to start thinking about how my other tools _should_ be used and then start using them that way.

[All Posts](/README.md)

[1]: https://github.com/pachun/boo
[2]: https://github.com/blakewilliams
