# vimake
 Vi's build system
```sh
# installation
git clone https://github.com/vivavy/vimake.git
cd vimake
sudo ./install
```

# Installation
```sh
git clone https://github.com/vivavy/vimake.git
cd vimake
sudo ./install
```

# Reference

All building units in ViMake are *modules*. Modules can be compiled separatly of have a depency one with other.
All modules also have build & run depencies. Usually it is compilers or interpreters. For each module you can
create one or more *implementations* (impl next times). When you running ViMake, you must specify your impl,
and interpreter (ipr next times) will execute all actions in this order: `build`, `run`, `debug`.
If no one action described in configuration, ipr will throw an error. usually only `build` action described.
If you want to create installation/deinstallation or any other script via ViMake, all commands you should
put into `run` action, not `build`. so, let's write simple example of configuration:

```yaml
module: main # here you need to put module name
impls: # dictionary with impls
    main: # impl "main"
        deps: # depencies for impl "main"
            build: # build depencies: compilers, etc.
                - g++
            modules: # modules must be compiled for this impl
                - test_module
        actions: # finally, actions
            build: # build action
                commands: # commands, each command is list of strings (arguments)
                    -   - g++ # first (and last :D) command
                        - -o
                        - test
                        - test.cc
                files: # output files. required for extended failure detection, if compiler doesn't return error code
                    - test
            run: # run action, executes after build action
                commands:
                    -   - ./test # just run output binary :D
```
