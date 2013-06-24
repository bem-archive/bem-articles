# smartcd, or how to use local installed tools from the commandline

Let us recall heart of a problem. We recommend to install all dependencies including [bem-tools](http://bem.info/tools/bem/) locally in the project. We describe them in `package.json` at the project root and install them by command `npm install`.

We would like to use locally installed tools in the project directory (at any level) just by calling their names: `bem make`, `borschik path/to/file.css`, etc.

Some of us are using the trick with adding to the environment variable PATH the value `./node_modules/.bin`. But this method works if only the command was run from the project root, and sometimes it's not convenient.

There is a better way to do it!

The tool [smartcd](https://github.com/cxreg/smartcd) allows to configure any directory in the way that every time when one enters it (or it's subdirectory) sertain commands will be executed or the environment variable will be set. We will use it!

The shortcut to set up everything looks as follows:

  - install `smartcd` (if you don't want to configure the smartcd, just answer the first question `Configure now? [Y/n]` — `N`)
    ```
    curl -L http://smartcd.org/install | bash
    ```

  - make template `npm-module` for `smartcd`
    ```
    smartcd template create npm-module
    ```

  - in the opened editor insert the line
    ```
    smartcd helper run path prepend __PATH__/node_modules/.bin
    ```
    after the comment: `# Enter any bash_enter commands below here: (leave this line!)`

  - set up the project:
    ```
    cd path/to/project
    echo 'smartcd template run npm-module' | smartcd edit enter
    ```

It's very usefull to set such alias for the shell and use it:

```
alias npm-smartcd="echo 'smartcd template run npm-module' | smartcd edit enter"
```

Enjoy!

```
~/projects$ which bem
/usr/local/bin/bem

~/projects$ cd bem-www
smartcd: running /Users/arikon/.smartcd/scripts/Users/arikon/projects/bem-www/bash_enter

~/projects/bem-www$ which bem
/Users/arikon/projects/bem-www/node_modules/.bin/bem

~/projects/bem-www$ cd blocks-desktop/

~/projects/bem-www/blocks-desktop$ which bem
/Users/arikon/projects/bem-www/node_modules/.bin/bem

~/projects/bem-www/blocks-desktop$ cd ../..

~/projects$ which bem
/Users/arikon/n/bin/bem
```

If you are using `zsh` with enabled option `autocd`, uncomment the line `smartcd setup prompt-hook` in `~/.smartcd_config`.

<!--(Begin) Article author block-->
<div class="article-author">
    <div class="article-author__photo">
        <img class="article-author__pictures" src="http://www.gravatar.com/avatar/6fa6da3a6927ded01bac659b5f1b4281.png?s=130" alt="Фотография Алексея Андросова">
    </div>
    <div class="article-author__info">
        <div class="article-author__row">
             <span class="article-author__name">Sergey Belov,
        </div>
        <div class="article-author__row">
          Yandex Dev.Tools Design Group Team Lead 
        </div>
        <div class="article-author__row">
             <a class="article-author__social-icon b-link" target="_blank" href="http://twitter.com/arik0n">twitter.com/arik0n</a>
        </div>
        <div class="article-author__row">
             <a class="article-author__social-icon b-link" target="_blank" href="http://github.com/arikon">github.com/arikon</a>
        </div>
    </div>
</div>
<!--(End) Article author block-->

This article is based on: «[Using the local installed tools from the comandline](http://clubs.ya.ru/bem/replies.xml?item_no=2231)» (Russian only) posted at Ya.ru.

