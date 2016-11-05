# run-in-terminal package

Atom package for run some commands in terminal, or just run terminal.

## Why?
Some packages can run terminal «here», some can run scripts not in terminal, but tabs/views/etc. I prefer terminal, so this one can run terminal «here» with any arguments and run scripts or any kind of shell «one-liners».

## Features
* start terminal here
* start terminal here and run some command
* start terminal here and run some command with extra arguments
* string interpolation with arguments
* understanding shebang (utf-8 only)
* launchers — file extension based command chooser
* separate context menus for tabs, tree and editor

## What's new ([changelog](https://github.com/pohmelie/run-in-terminal/blob/master/CHANGELOG.md))
#### 0.6.0 - run with arguments
* feature: start terminal and run with extra arguments

## Options

| Field                          |   Type  |                Description                               |        Default value             |                 Example value                   |
|:------------------------------:|:-------:|:--------------------------------------------------------:|:--------------------------------:|:-----------------------------------------------:|
| Terminal                       | string  | command to start terminal with argumenst                 | your-favorite-terminal arguments | konsole --noclose --workdir {working_directory} |
| Terminal execution argument    | string  | argument to run some command in terminal                 | terminal-execution-argument      | -e                                              |
| List of launchers by extension | string  | comma separated pairs: extension-launcher                | your-launchers                   | .py python3 {file_path}, .lua lua {file_path}   |
| Save file before run terminal  | boolean |                                                          | true                             | true                                            |
| Use exec cwd                   | boolean | child_process.exec cwd parameter                         | true                             | true                                            |
| Use shebang                    | boolean | use shebang if available                                 | true                             | true                                            |
| Autoquotation                  | boolean | adding double quotation mark to interpolation parameters | true                             | true                                            |

#### Windows users may use «start» command with «cmd»:

    start /D {working_directory} C:\Windows\System32\cmd.exe /u

and «terminal execution argument»:

    /k

#### Mac users may use «open» command with their favorite terminal app:

    open -a /path/to/terminal.app

## Interpolation parameters
| Parameter           | Description                       |
|:-------------------:|:---------------------------------:|
| {file_path}         | path to current file              |
| {working_directory} | path to current working directory |
| {project_directory} | path to project's root directory  |
| {git_directory}     | path to nearest git root directory|

## How it works
In deep, run-in-terminal use node.js child_process.exec function, so exec have cwd (current working directory) argument. But it doesn't works for any terminal. Some of them need launch «working directory» argument. That's why run-in-terminal have string interpolation of arguments. What is string interpolation means? run-in-terminal build full command at first step and replace predefined substrings with parameters at second. For values from «example value» column above we can have such scenario: opened /path/to/somedir/foo.py, which have #!/usr/bin/python3 shebang

    start-terminal-here-and-run -> konsole --noclose --workdir {working_directory} -e /usr/bin/python3 {file_path}

this will be interpolated to:

    start-terminal-here-and-run -> konsole --noclose --workdir "/path/to/somedir" -e /usr/bin/python3 "/path/to/somedir/foo.py"

If run-in-terminal can't determine launcher or file_path (file not saved and have no name) it will do start-terminal-here.

## A real life example
MacOS ver. 10.12.1, runing Atom ver. 1.11.2, using run-in-terminal ver. 0.6.0. The tested program is called test.py and in order to run the script successfully, one first change the python script into excutable:

chmod +x test.py

Then, one has to add the shebang into the first line of the python script:

 \#!/usr/bin/env python

Then one has to configure in the Settings:

1. In 'List of launchers by extension': 

    .py {file_path}

2. In 'Terminal with argument':

 open -a /Applications/Utilities/Terminal.app {working_directory}
 
    or, if just want to run script, then you can suppress the extra terminal by deleting {working_directory}:
 
    open -a /Applications/Utilities/Terminal.app

3. In 'Terminal execution argument':

    -e

4. Unselect 'Use shebang if available'.

*Notice that Settings only need to configure once. But every test.py files has to be changed into excutable and added shebang.

*Notice 'konsole --noclose --workdir' didn't work in this test case.

## Thanks to:
[bobrocke](https://github.com/bobrocke), [clintwood](https://github.com/clintwood), [LeoVerto](https://github.com/LeoVerto), [marales](https://github.com/marales), [djengineerllc](https://github.com/djengineerllc), [LevPasha](https://github.com/LevPasha)
