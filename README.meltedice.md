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
        for rcfile in "${ZDOTDIR:-$HOME}"/.zprezto/runcoms/^README.md(.N); do; ln -snf "$rcfile" "${ZDOTDIR:-$HOME}/.${rcfile:t}"; done

  5. Install powerline fonts

        brew reinstall --powerline --vim-powerline ricty
        rehash
        cp /usr/local/Cellar/ricty/3.2.4/share/fonts/Ricty*.ttf /Library/Fonts
        % fc-cache -vf

  6. Change iTerm.app preferences to use "Ricty for Powerline"

  7. Set Zsh as your default shell:

        chsh -s /bin/zsh

  8. Open a new Zsh terminal window or tab.

Updating
--------

Pull the latest changes from upstream and update submodules.

    git status
    [ "$optional" = "" ] && git stash
    git checkout master
    git pull --rebase upstream master
    git push
    git checkout meltedice
    git rebase master
    git submodule update --init --recursive
    git push -f
    [ "$optional" = "" ] && git stash apply

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
