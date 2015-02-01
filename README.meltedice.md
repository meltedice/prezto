Prezto — Instantly Awesome Zsh (for meltedice)
==========================================

[Prezto](https://github.com/sorin-ionescu/prezto) with meltedice's configuration.

Installation
------------


  1. Launch Zsh:

        zsh

  2. Clone my repository:

        git clone --recursive git@github.com/meltedice/prezto.git "${ZDOTDIR:-$HOME}/.zprezto"

  3. Set [upstream](https://github.com/sorin-ionescu/prezto) and switch to my branch [meltedice](https://github.com/sorin-ionescu/prezto.git):

        cd "${ZDOTDIR:-$HOME}/.zprezto"
        git remote add upstream https://github.com/sorin-ionescu/prezto.git
        git checkout meltedice
        cd

  4. Create a new Zsh configuration by copying the Zsh configuration files
     provided:

        setopt EXTENDED_GLOB
        for rcfile in "${ZDOTDIR:-$HOME}"/.zprezto/runcoms/^README.md(.N); do
          ln -s "$rcfile" "${ZDOTDIR:-$HOME}/.${rcfile:t}"
        done

  5. Set Zsh as your default shell:

        chsh -s /bin/zsh

  6. Open a new Zsh terminal window or tab.

Updating
--------

Pull the latest changes from upstream and update submodules.

    git status
    git stash
    git checkout master
    git pull --rebase upstream master
    git submodule update --init --recursive
    git push
    git checkout meltedice
    git rebase master
    git push

Usage
-----

See [Prezto](https://github.com/sorin-ionescu/prezto)

Customization
-------------

See [Prezto](https://github.com/sorin-ionescu/prezto)

Resources
---------

See [Prezto](https://github.com/sorin-ionescu/prezto)

License
-------

See [Prezto](https://github.com/sorin-ionescu/prezto)

(The MIT License)
