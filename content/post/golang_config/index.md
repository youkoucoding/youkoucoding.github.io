---
title: 'Install Go on Mac with Homebrew'
date: 2021-09-07T23:46:37+09:00
slug: 'golang-config'
image: 'cover.jpg'
categories:
  - 'Dev Environment'
tag:
  - fish shell
  - macOs
  - Golang
---

> Precondition: macOS Big Sur 11.5.2

## Golang's installation and configuration with homebrew in Fish shell

### 1. Install golang with homebrew

```fish
brew update
brew search golang
brew info golang # show the information of golang
brew install golang
```

### 2. Setup the workspace:

#### Add environment variables:

First, we'll need to tell Go the location of our workspace.

We'll add some environment variables into shell config.

##### `bash or zsh`

The config files is located at home directory:

`.bash_profile, bashrc, or zshrc`

```bash
export GOPATH=$HOME/project/go-workspace # don't forget to change the path correctly
export GOROOT=/usr/local/opt/go/libexec
# export GOROOT=/usr/local/Cellar/go/1.17/libexec
export PATH=$PATH:$GOPATH/bin
export PATH=$PATH:$GOROOT/bin
```

> `GOPATH` is the place where we get, build and install packages outside the standard Go tree(AND it's not where the executables are), so it is customizable and thus you can get `GOPATH` wherever you want.

> `GOROOT`, however, is the place where the Go binary distributions (In linux, it's normally in `/usr/local/go`; but for macOs, the Go and the tools are installed in `/usr/local/Cellar/go/1.17/libexec` by default as well as a soft link to `/usr/local/opt/go/libexec` will be created at the same time), so on Linux normally we don’t have to set this variable, but on MacOS, we have to change it as brew installs Go tools to a different path.

##### `fish shell`

Add the code to home directory's `~/.config/fish/config.fish`:

```fish
set -x GOPATH $HOME/project/go-workspace
set -x GOROOT /usr/local/opt/go/libexec
set -x PATH $PATH:$GOPATH/bin
set -x PATH $PATH:$GOROOT/bin
```

#### Create our own workspace:

`mkdir -p $GOPATH $GOPATH/src $GOPATH/pkg $GOPATH/bin`

- `$GOPATH/src`: where we put the go projects source code

- `$GOPATH/pkg`: contains every package objects

- `$GOPATH/bin`: The compiled binaries's home.
