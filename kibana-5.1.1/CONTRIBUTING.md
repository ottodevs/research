# Contributing to Kibana

We understand that you may not have days at a time to work on Kibana. We ask that you read our contributing guidelines carefully so that you spend less time, overall, struggling to push your PR through our code review processes.

At the same time, reading the contributing guidelines will give you a better idea of how to post meaningful issues that will be more easily be parsed, considered, and resolved. A big win for everyone involved! :tada:

## Table of Contents

A high level overview of our contributing guidelines.

- [Effective issue reporting in Kibana](#effective-issue-reporting-in-kibana)
  - [Voicing the importance of an issue](#voicing-the-importance-of-an-issue)
  - ["My issue isn't getting enough attention"](#my-issue-isnt-getting-enough-attention)
  - ["I want to help!"](#i-want-to-help)
- [How We Use Git and GitHub](#how-we-use-git-and-github)
  - [Branching](#branching)
  - [Commits and Merging](#commits-and-merging)
  - [What Goes Into a Pull Request](#what-goes-into-a-pull-request)
- [Contributing Code](#contributing-code)
  - [Setting Up Your Development Environment](#setting-up-your-development-environment)
    - [Customizing `config/kibana.dev.yml`](#customizing-configkibanadevyml)
    - [Setting Up SSL](#setting-up-ssl)
  - [Linting](#linting)
  - [Testing and Building](#testing-and-building)
  - [Debugging Unit Tests](#debugging-unit-tests)
  - [Unit Testing Plugins](#unit-testing-plugins)
  - [Cross-browser compatibility](#cross-browser-compatibility)
    - [Testing compatibility locally](#testing-compatibility-locally)
    - [Running Browser Automation Tests](#running-browser-automation-tests)
      - [Browser Automation Notes](#browser-automation-notes)
  - [Building OS packages](#building-os-packages)
- [Signing the contributor license agreement](#signing-the-contributor-license-agreement)
- [Submitting a Pull Request](#submitting-a-pull-request)
- [Code Reviewing](#code-reviewing)
  - [Getting to the Code Review Stage](#getting-to-the-code-review-stage)
  - [Reviewing Pull Requests](#reviewing-pull-requests)

Don't fret, it's not as daunting as the table of contents makes it out to be!

## Effective issue reporting in Kibana

At any given time the Kibana team at Elastic is working on dozens of features and enhancements, both for Kibana itself and for a few other projects at Elastic. When you file an issue, we'll take the time to digest it, consider solutions, and weigh its applicability to both the Kibana user base at large and the long-term vision for the project. Once we've completed that process, we will assign the issue a priority.

- **P1**: A high-priority issue that affects virtually all Kibana users. Bugs that would cause incorrect results, security issues and features that would vastly improve the user experience for everyone. Work arounds for P1s generally don't exist without a code change.
- **P2**: A broadly applicable, high visibility, issue that enhances the usability of Kibana for a majority users.
- **P3**: Nice-to-have bug fixes or functionality. Work arounds for P3 items generally exist.
- **P4**: Niche and special interest issues that may not fit our core goals. We would take a high quality pull for this if implemented in such a way that it does not meaningfully impact other functionality or existing code. Issues may also be labeled P4 if they would be better implemented in Elasticsearch.
- **P5**: Highly niche or in opposition to our core goals. Should usually be closed. This doesn't mean we wouldn't take a pull for it, but if someone really wanted this they would be better off working on a plugin. The Kibana team will usually not work on P5 issues but may be willing to assist plugin developers on IRC.

### Voicing the importance of an issue

We seriously appreciate thoughtful comments. If an issue is important to you, add a comment with a solid write up of your use case and explain why it's so important. Please avoid posting comments comprised solely of a thumbs up emoji 👍.

Granted that you share your thoughts, we might even be able to come up with creative solutions to your specific problem. If everything you'd like to say has already been brought up but you'd still like to add a token of support, feel free to add a [👍 thumbs up reaction](https://github.com/blog/2119-add-reactions-to-pull-requests-issues-and-comments) on the issue itself and on the comment which best summarizes your thoughts.

### "My issue isn't getting enough attention"

First of all, **sorry about that!** We want you to have a great time with Kibana.

Hosting meaningful discussions on GitHub can be challenging. For that reason, we'll sometimes ask that you join us on IRC _([#kibana](https://kiwiirc.com/client/irc.freenode.net/?#kibana) on freenode)_ to chat about your issues. You may also experience **faster response times** when engaging us via IRC.

There's hundreds of open issues and prioritizing what to work on is an important aspect of our daily jobs. We prioritize issues according to impact and difficulty, so some issues can be neglected while we work on more pressing issues.

Feel free to bump your issues if you think they've been neglected for a prolonged period, or just jump on IRC and let us have it!

### "I want to help!"

**Now we're talking**. If you have a bug fix or new feature that you would like to contribute to Kibana, please **find or open an issue about it before you start working on it.** Talk about what you would like to do. It may be that somebody is already working on it, or that there are particular issues that you should know about before implementing the change.

We enjoy working with contributors to get their code accepted. There are many approaches to fixing a problem and it is important to find the best approach before writing too much code.

## How We Use Git and GitHub

### Branching

* All work on the next major release goes into master.
* Past major release branches are named `{majorVersion}.x`. They contain work that will go into the next minor release. For example, if the next minor release is `5.2.0`, work for it should go into the `5.x` branch.
* Past minor release branches are named `{majorVersion}.{minorVersion}`. They contain work that will go into the next patch release. For example, if the next patch release is `5.3.1`, work for it should go into the `5.3` branch.
* All work is done on feature branches and merged into one of these branches.
* Where appropriate, we'll backport changes into older release branches.

### Commits and Merging

* Feel free to make as many commits as you want, while working on a branch.
* When submitting a PR for review, please perform an interactive rebase to present a logical history that's easy for the reviewers to follow.
* Please use your commit messages to include helpful information on your changes, e.g. changes to APIs, UX changes, bugs fixed, and an explanation of *why* you made the changes that you did.
* Resolve merge conflicts by rebasing the target branch over your feature branch, and force-pushing.
* When merging, we'll squash your commits into a single commit.

### What Goes Into a Pull Request

* Please include an explanation of your changes in your PR description.
* Links to relevant issues, external resources, or related PRs are very important and useful.
* Please update any tests that pertain to your code, and add new tests where appropriate.
* See [Submitting a Pull Request](#submitting-a-pull-request) for more info.

## Contributing Code

These guidelines will help you get your Pull Request into shape so that a code review can start as soon as possible.

### Setting Up Your Development Environment

Clone the `kibana` repo and change directory into it

```bash
git clone https://github.com/elastic/kibana.git kibana
cd kibana
```

Install the version of node.js listed in the `.node-version` file _(this can be easily automated with tools such as [nvm](https://github.com/creationix/nvm) and [avn](https://github.com/wbyoung/avn))_

```bash
nvm install "$(cat .node-version)"
```

Install `npm` dependencies

```bash
npm install
```

Start elasticsearch.

```bash
npm run elasticsearch
```

> You'll need to have a `java` binary in `PATH` or set `JAVA_HOME`.

If you're just getting started with `elasticsearch`, you could use the following command to populate your instance with a few fake logs to hit the ground running.

```bash
npm run makelogs
```

> Make sure to execute `npm run makelogs` *after* elasticsearch is up and running!

Start the development server.
  ```bash
  npm start
  ```

> On Windows, you'll need you use Git Bash, Cygwin, or a similar shell that exposes the `sh` command.  And to successfully build you'll need Cygwin optional packages zip, tar, and shasum.

#### Customizing `config/kibana.dev.yml`

The `config/kibana.yml` file stores user configuration directives. Since this file is checked into source control, however, developer preferences can't be saved without the risk of accidentally committing the modified version. To make customizing configuration easier during development, the Kibana CLI will look for a `config/kibana.dev.yml` file if run with the `--dev` flag. This file behaves just like the non-dev version and accepts any of the [standard settings](https://www.elastic.co/guide/en/kibana/master/kibana-server-properties.html).

The `config/kibana.dev.yml` file is very commonly used to store some opt-in/**unsafe** optimizer tweaks which can significantly increase build performance. Below is a commonly used `config/kibana.dev.yml` file, but additional options can be found [in #4611](https://github.com/elastic/kibana/pull/4611#issue-99706918).

```yaml
optimize:
  sourceMaps: '#cheap-source-map' # options -> http://webpack.github.io/docs/configuration.html#devtool
  unsafeCache: true
  lazyPrebuild: false
```

#### Setting Up SSL

When Kibana runs in development mode it will automatically use bundled SSL certificates. These certificates won't be trusted by your OS by default which will likely cause your browser to complain about the certificate.

If you run into this issue, visit the development server and configure your OS to trust the certificate.

- OSX: https://www.accuweaver.com/2014/09/19/make-chrome-accept-a-self-signed-certificate-on-osx/
- Windows: http://stackoverflow.com/a/1412118
- Linux: http://unix.stackexchange.com/a/90607

There are a handful of other options, although we enthusiastically recommend that you trust our development certificate.

- Click through the warning and accept future warnings
- Supply your own certificate using the `config/kibana.dev.yml` file
- Disable SSL in Kibana by starting the application with `npm start -- --no-ssl`

### Linting

A note about linting: We use [eslint](http://eslint.org) to check that the [styleguide](STYLEGUIDE.md) is being followed. It runs in a pre-commit hook and as a part of the tests, but most contributors integrate it with their code editors for real-time feedback.

Here are some hints for getting eslint setup in your favorite editor:

Editor     | Plugin
-----------|-------------------------------------------------------------------------------
Sublime    | [SublimeLinter-eslint](https://github.com/roadhump/SublimeLinter-eslint#installation)
Atom       | [linter-eslint](https://github.com/AtomLinter/linter-eslint#installation)
IntelliJ   | Settings » Languages & Frameworks » JavaScript » Code Quality Tools » ESLint
`vi`       | [scrooloose/syntastic](https://github.com/scrooloose/syntastic)

Another tool we use for enforcing consistent coding style is EditorConfig, which can be set up by installing a plugin in your editor that dynamically updates its configuration. Take a look at the [EditorConfig](http://editorconfig.org/#download) site to find a plugin for your editor, and browse our [`.editorconfig`](https://github.com/elastic/kibana/blob/master/.editorconfig) file to see what config rules we set up.

### Testing and Building

To ensure that your changes will not break other functionality, please run the test suite and build process before submitting your Pull Request.

Before running the tests you will need to install the projects dependencies as described above.

Once that's done, just run:

```bash
npm run test && npm run build -- --skip-os-packages
```

### Debugging Unit Tests

The standard `npm run test` task runs several sub tasks and can take several minutes to complete, making debugging failures pretty painful. In order to ease the pain specialized tasks provide alternate methods for running the tests.

To execute both server and browser tests, but skip linting, use `npm run test:quick`.

```bash
npm run test:quick
```

Use `npm run test:server` when you want to run only the server tests.

```bash
npm run test:server
```

When you'd like to execute individual server-side test files, you can use the command below. Note that this command takes care of configuring Mocha with Babel compilation for you, and you'll be better off avoiding a globally installed `mocha` package. This command is great for development and for quickly identifying bugs.

```bash
npm run mocha <file>
```

You could also add the `:debug` target so that `node` is run using the `--debug-brk` flag. You'll need to connect a remote debugger such as [`node-inspector`](https://github.com/node-inspector/node-inspector) to proceed in this mode.

```bash
npm run mocha:debug <file>
```

With `npm run test:browser`, you can run only the browser tests. Coverage reports are available for browser tests by running `npm run test:coverage`. You can find the results under the `coverage/` directory that will be created upon completion.

```bash
npm run test:browser
```

Using `npm run test:dev` initializes an environment for debugging the browser tests. Includes an dedicated instance of the kibana server for building the test bundle, and a karma server. When running this task the build is optimized for the first time and then a karma-owned instance of the browser is opened. Click the "debug" button to open a new tab that executes the unit tests.

```bash
npm run test:dev
```

![Browser test debugging](http://i.imgur.com/DwHxgfq.png)

### Unit Testing Plugins

This should work super if you're using the [Kibana plugin generator](https://github.com/elastic/generator-kibana-plugin). If you're not using the generator, well, you're on your own. We suggest you look at how the generator works.

To run the tests for just your particular plugin, assuming you plugin lives outside of the `plugins directory`, use the following command.

```bash
npm run test:dev -- --kbnServer.testsBundle.pluginId=some_special_plugin --kbnServer.plugin-path=../some_special_plugin
```

### Cross-browser Compatibility

#### Testing Compatibility Locally

##### Testing IE on OS X

* [Download VMWare Fusion](http://www.vmware.com/products/fusion/fusion-evaluation.html).
* [Download IE virtual machines](https://developer.microsoft.com/en-us/microsoft-edge/tools/vms/#downloads) for VMWare.
* Open VMWare and go to Window > Virtual Machine Library. Unzip the virtual machine and drag the .vmx file into your Virtual Machine Library.
* Right-click on the virtual machine you just added to your library and select "Snapshots...", and then click the "Take" button in the modal that opens. You can roll back to this snapshot when the VM expires in 90 days.
* In System Preferences > Sharing, change your computer name to be something simple, e.g. "computer".
* Run Kibana with `npm start -- --no-ssl --host=computer.local` (subtituting your computer name).
* Now you can run your VM, open the browser, and navigate to `http://computer.local:5601` to test Kibana.

#### Running Browser Automation Tests

The following will start Kibana, Elasticsearch and the chromedriver for you. To run the functional UI tests use the following commands

If you want to run the functional UI tests one time and exit, use the following command. This is used by the CI systems and is great for quickly checking that things pass. It is essentially a combination of the next two tasks.  This supports options `--grep=foo` for only running tests that match a regular expression, and `--appSuites=management` for running tests for a specific application.

```bash
npm run test:ui
```


In order to start the server required for the `test:ui:runner` tasks, use the following command. Once the server is started `test:ui:runner` can be run multiple times without waiting for the server to start.

```bash
npm run test:ui:server
```

To execute the front-end browser tests, enter the following. This requires the server started by the `test:ui:server` task.

```bash
npm run test:ui:runner
```

To run these browser tests against against some other Elasticsearch and Kibana instance you can set these environment variables and then run the test runner.
Here's an example to run against an Elastic Cloud instance (note that you should run the same branch of tests as the version of Kibana you're testing);

```bash
export TEST_KIBANA_PROTOCOL=https
export TEST_KIBANA_HOSTNAME=9249d04b1186b3e7bbe11ea60df4f963.us-east-1.aws.found.io
export TEST_KIBANA_PORT=443
export TEST_KIBANA_USER=elastic
export TEST_KIBANA_PASS=<your password here>

export TEST_ES_PROTOCOL=http
export TEST_ES_HOSTNAME=aaa5d22032d76805fcce724ed9d9f5a2.us-east-1.aws.found.io
export TEST_ES_PORT=9200
export TEST_ES_USER=elastic
export TEST_ES_PASS=<your password here>
npm run test:ui:runner
```

##### Browser Automation Notes

- Using Page Objects pattern (https://theintern.github.io/intern/#writing-functional-test)
- At least the initial tests for the Settings, Discover, and Visualize tabs all depend on a very specific set of logstash-type data (generated with makelogs).  Since that is a static set of data, all the Discover and Visualize tests use a specific Absolute time range.  This guarantees the same results each run.
- These tests have been developed and tested with Chrome and Firefox browser.  In theory, they should work on all browsers (that's the benefit of Intern using Leadfoot).
- These tests should also work with an external testing service like https://saucelabs.com/ or https://www.browserstack.com/ but that has not been tested.
- https://theintern.github.io/
- https://theintern.github.io/leadfoot/module-leadfoot_Element.html

### Building OS packages

Packages are built using fpm, pleaserun, dpkg, and rpm.  fpm and pleaserun can be installed using gem.  Package building has only been tested on Linux and is not supported on any other platform.

```bash
apt-get install ruby-dev rpm
gem install fpm -v 1.5.0
gem install pleaserun -v 0.0.24
npm run build -- --skip-archives
```

To specify a package to build you can add `rpm` or `deb` as an argument.

```bash
npm run build -- --rpm
```

Distributable packages can be found in `target/` after the build completes.

## Signing the contributor license agreement

Please make sure you have signed the [Contributor License Agreement](http://www.elastic.co/contributor-agreement/). We are not asking you to assign copyright to us, but to give us the right to distribute your code without restriction. We ask this of all contributors in order to assure our users of the origin and continuing existence of the code. You only need to sign the CLA once.

## Submitting a Pull Request

Push your local changes to your forked copy of the repository and submit a Pull Request. In the Pull Request, describe what your changes do and mention the number of the issue where discussion has taken place, eg “Closes #123″.

Always submit your pull against `master` unless the bug is only present in an older version. If the bug effects both `master` and another branch say so in your pull.

Then sit back and wait. There will probably be discussion about the Pull Request and, if any changes are needed, we'll work with you to get your Pull Request merged into Kibana.

## Code Reviewing

After a pull is submitted, it needs to get to review. If you have commit permission on the Kibana repo you will probably perform these steps while submitting your Pull Request. If not, a member of the Elastic organization will do them for you, though you can help by suggesting a reviewer for your changes if you've interacted with someone while working on the issue.

### Getting to the Code Review Stage

1. Assign the `review` label. This signals to the team that someone needs to give this attention.
1. Do **not** assign a version label. Someone from Elastic staff will assign a version label, if necessary, when your Pull Request is ready to be merged.
1. Find someone to review your pull. Don't just pick any yahoo, pick the right person. The right person might be the original reporter of the issue, but it might also be the person most familiar with the code you've changed. If neither of those things apply, or your change is small in scope, try to find someone on the Kibana team without a ton of existing reviews on their plate. As a rule, most pulls will require 2 reviewers, but the first reviewer will pick the 2nd.

### Reviewing Pull Requests

So, you've been assigned a pull to review. What's that look like?

Remember, someone is blocked by a pull awaiting review, make it count. Be thorough, the more action items you catch in the first review, the less back and forth will be required, and the better chance the pull has of being successful.

1. **Understand the issue** that is being fixed, or the feature being added. Check the description on the pull, and check out the related issue. If you don't understand something, ask the submitter for clarification.
1. **Reproduce the bug** (or the lack of feature I guess?) in the destination branch, usually `master`. The referenced issue will help you here. If you're unable to reproduce the issue, contact the issue submitter for clarification
1. **Check out the pull** and test it. Is the issue fixed? Does it have nasty side effects? Try to create suspect inputs. If it operates on the value of a field try things like: strings (including an empty string), null, numbers, dates. Try to think of edge cases that might break the code.
1. **Merge the target branch**. It is possible that tests or the linter have been updated in the target branch since the pull was submitted. Merging the pull could cause core to start failing.
1. **Read the code**. Understanding the changes will help you find additional things to test. Contact the submitter if you don't understand something.
1. **Go line-by-line**. Are there [style guide](https://github.com/elastic/kibana/blob/master/STYLEGUIDE.md) violations? Strangely named variables? Magic numbers? Do the abstractions make sense to you? Are things arranged in a testable way?
1. **Speaking of tests** Are they there? If a new function was added does it have tests? Do the tests, well, TEST anything? Do they just run the function or do they properly check the output?
1. **Suggest improvements** If there are changes needed, be explicit, comment on the lines in the code that you'd like changed. You might consider suggesting fixes. If you can't identify the problem, animated screenshots can help the review understand what's going on.
1. **Hand it back** If you found issues, re-assign the submitter to the pull to address them. Repeat until mergable.
1. **Hand it off** If you're the first reviewer and everything looks good but the changes are more than a few lines, hand the pull to someone else to take a second look. Again, try to find the right person to assign it to.
1. **Merge the code** When everything looks good, put in a `LGTM` (looks good to me) comment. Merge into the target branch. Check the labels on the pull to see if backporting is required, and perform the backport if so.

Thank you so much for reading our guidelines! :tada:
