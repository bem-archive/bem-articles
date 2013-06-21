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

  - make template npm-module for smartcd
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
