# Lua Changed My Life

[Lua](2) changed my life.

I love [Ruby](3) because it's with a doubt the nicest programming language I've ever worked with.

I have a personal interest in [Elixir](4) and I'm using it to build an api for a personal project because I like functional programming concepts and the niceties that come with using [Phoenix](5).

Professionally, I've been using [React](6) for a decade (and exclusively for the past 5 years) because it made front end development tolerable in my opinion.

I also love making iPhone Apps and prior to the advent of [React Native](7), I had a torrid affair with [RubyMotion](8). I have used React Native ([and, importantly, Expo](9)) for several client projects and just about every single side project I've made since around 2017.

I don't like python.

Regardless of which programming language I'm using, though, there are three tools that I've used (almost) every day for well over a decade (~13 years): [Vim](10) (Replaced, for me, by [Neovim](13)), [Git](11) and [Tmux](12).

About a year ago, I learned that lua had become Neovim's default configuration language. [Since then, my dotfiles have evolved to place that I'm actually proud of](14).

Before Lua, I floudnered to configure Vim with VimScript. It felt arcane. I misused tmux to compensate. I disappointed [friends and colleagues](https://github.com/blakewilliams) with my abuse of tools they loved.

After Lua, I stopped using tmux panes for _everything_ (I had been opening a vim instance in each tmux pane to see all the different relevant bits of a bunch of files at once).

I configured vim panes to work the way I'd been abusing tmux panes (hotkey-wise), to ease the transition and take advantage of vim, because Lua made it easy to configure.

I quickly stopped using the tmux-to-vim-pane crutch that I created because ... I installed an LSP ... because Lua & Lazy.nvim made it easy to install and configure and jumping directly to the definition of somthing, and then right back out to where you had been is _so_ much better than opening a tmux pane and new vim instance.

I stopped typing `vim full/file/path.rb` because I installed telescope, because, again lua & Lazy.nvim made it easy to install and configure.

Sometimes I still use splits. But they're vim splits. Not tmux panes for multiple vim instances. And I had been doing that for a decade! In a couple of months after lua came out, I was using all the tools I'd been using prior, but with the confidence to configure the most capable one: my editor. And I was using them all very differently.

I also still use tmux. I think I'm using it for what it's supposed to be used for ... for the first time. And it's !@#$ing great!

![tmux](/assets/2025-03-18-lua-changed-my-life/tmux.png)

I feel like being able to configure what had seemed like the most complicated peice of my dotfiles, started this avalanche of learning that led me to learning so much more about the tools I've always used, enabled me to not only start using them correctly, but configure and customize them and really organize my development environment to a point where I usually feel way more focused. And when I need to unfocus on something and focus on something else really quickly, tmux plops me right where I need to be in less than 2 seconds.

[All Posts](1)

[1]: /README.md
[2]: https://www.lua.org
[3]: https://www.ruby-lang.org
[4]: https://elixir-lang.org
[5]: https://phoenixframework.org
[6]: https://reactjs.org
[7]: https://reactnative.dev
[8]: https://rubymotion.com
[9]: https://expo.io
[10]: https://www.vim.org
[11]: https://git-scm.com
[12]: https://github.com/tmux/tmux
[13]: https://neovim.io
[14]: https://github.com/pachun/boo
