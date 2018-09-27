# sitespeed.io 'local' documentation / simple usage

Sitespeed.io is a set of Open Source tools that makes it easy to monitor and measure the performance of your web site.

## Getting Started

This instructions will get you started with simple usage of sitespeed.io tools.

### Prerequisites

What things you need to install the tool and how to install them:

Sitespeed.io runs inside ***Docker container***, so you need Docker to get into this environment:
1. Latest version of Docker - [get it from here](https://docs.docker.com/install/):
* Choose OS that you use from navigation bar and scroll to part saying "Install and run Docker for Mac/ Windows/ Linux".
* Follow instructions on 'how to install' on Docker page depending on OS that you choose.

2. Clone this repository - `git clone git@github.com:degordian/qa-fortress.git`

3. Place yourself in `qa-fortress/Examples/sitespeed_test_template/` folder and run `npm install` to install dependencies (actually just gpsi plug-in that's explained below). Plugin is gonna be placed inside `./node_modules/@sitespeed.io/plugin-gpsi` directory.

### Installing
If you use ***macOS***:
1. Run Docker (it's probably stored inside your `/Applications` folder).
2. Inspect content of sitespeed_test_template folder:
* `config.json` - all configuration for sitespeed stored inside one json file, learn more about sitespeed.io config from [here](https://www.sitespeed.io/documentation/sitespeed.io/configuration/).
* `plugin-gpsi` folder - "gpsi" stands for Google's 'PageSpeed Insights'; therefor this is plugin that connects sitespeed.io with Google's PageSpeed Insight criteria for even robust performance score, more about that from [here](https://github.com/sitespeedio/plugin-gpsi).

***Important!***

You need to place you API key inside CLI call that we'r gonna discuss after this. Read how to find you gpsi key from Google [here](https://cloud.google.com/docs/authentication/api-keys?visit_id=636729451490184021-594932787&rd=1).

* `runSitespeed.sh` - CLI (command language interpreter) script; with instructions for sitespeed.io on how to run performance tests on you website:

`docker run --shm-size=1g --rm -v "$(pwd)":/sitespeed.io sitespeedio/sitespeed.io ‘URL’` (***simplest way***)

In our example it's like this:

`docker run --shm-size=1g --rm -v "$(pwd)":/sitespeed.io sitespeedio/sitespeed.io https://cultofthepartyparrot.com/ --plugins.add plugin-gpsi --gpsi.key AIzaSyBxbOnkaOtuAmKJrF5d9zwLN7AofyQ1s9o --budget.configPath config.json --budget.output junit -b chrome -n 1 -d 2 --metrics.filterList --metrics.list`

Where: 

`docker run --shm-size=1g --rm -v` and part `:/sitespeed.io sitespeedio/sitespeed.io` - are instructions for Docker to run sitespeed.io container.

`"$(pwd)"` - as if current directory (print current directory command).

`plugins.add node_modules/@sitespeed.io/plugin-gpsi --gpsi.key your_API_key` - chosen plugin, it's directory location, and API key. See how to find your API here in above description.

`https://cultofthepartyparrot.com/` - or any other `URL` of website of you choose - here you place `URL` of website you would like to ***test for performance***.

Also you can add multiple URLs, just place them one after other, for example like this: 
`https://cultofthepartyparrot.com/ https://en.wikipedia.org/wiki/5_Whys` and so on.

`budget.configPath config.json` - location of config file, in this case it is in same folder as run skript.

`budget.output junit` - creates `junit` file that can later be user via Jenkins CI tool for example.

`b chrome` - `b` stands for `browser`, in this case Chrome browser.

`n 1 -d 2` - `n` = `number of iteration`; `d` = `crawler depth`; more about that in [here](https://www.sitespeed.io/documentation/sitespeed.io/configuration/) "The options" section.

`metrics.filterList --metrics.list` - `metrics.filerList` = generates list of all filters places inside metric/`config file`; `metriclist` = generates list of all available filters; all that can be seen inside `Data` folder that will be generated after script is executed.

If you use ***Windows***:

There are few differences:
1. Run Docker, but:
* Choose "Linux container" - right click on running Docker icon and choose "Switch to Linux containers". Long story short, from my experience that was the only way for me to run sitespeed.io on Win OS.
2. From `cmd` (cmd.exe/ Command Prompt) directly, choose to ***run like administrator*** this command (same as one from above):
`docker run --shm-size=1g --rm -v “$(absolute path*)”:/sitespeed.io sitespeedio/sitespeed.io http://www.deghq.com/autohrvatska-feature-staging/ --plugins.add plugin-gpsi --gpsi.key AIzaSyBxbOnkaOtuAmKJrF5d9zwLN7AofyQ1s9o --budget.configPath config.json --budget.output junit -b chrome -n 1 -d 2 --metrics.filterList --metrics.list`

Take a look at difference:
`“$(absolute path*)”` insted of `"$(pwd)"` - place here ***apsolute path*** to you sitespeed.io folder.
For example:
`“C:\Users\admin\Desktop\Sitespeed test template”`

## How to run
Example from macOS machine:
1. Place yourself inside `Sitespeed test template` folder and run command `sh runSitespeed.sh` (I'm using bash shell inside tool iTerm2).
2. Wait for some time depending on parameters `n` and `d` inside your script.
3. That's it. :)

Now you can see generated folder called `sitespeed-result` inside folder you were placed before executing script, and inside is another folder called after `URL` chosen in your performance test, with metrics and results in HTML format, with all data nicely visualized. :) 
For every new execution of script you will get new folders named after URLs and time when it was finished.

So directory structure is like this (for our example):
`Sitespeed test template` folder -> `sitespeed result` folder -> folder named after `URL/URLs` -> folder named after `time and date` of execution -> result of performace test `(HTML files)`, `junit.xml` as in Jenkins file, `Data` folder with all metrics, and so on.

***Happy testing folks!***

Please contact me in case of any misunderstanding - vanja.zunic@bornfight.com / zunicc@gmail.com

