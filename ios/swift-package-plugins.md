# Swift Package Plugin
I was developing my Network Layer written in swift, and I wanted to localize some of API Errors that I handlded, and then I thought that I could introduce in my spm package the famous code generator Swiftgen, but then I have faced a problem:
Until swift 5.6, I have no way to add swiftgen to a spm package. I have to manually create my Strings enum from a Localizable file. But this have change at WWDC22, that introduced the newly Swift Package Build Plugin API. Now I'm able to create a custom plugin to adding custom steps in the build and pre-build phase.

## What is a package plugin

### Types of plugins


#### Command plugin

Command plugin implement development time actions, applied directly to a package, not during build

- Command plugin: implement custom actions -> can run linter or source code formatter, run scripts like build phase
- generally really small and depend of other tools to do the work
- xcode will build any binaries dependencies before the command is invoked

```bash
PACKAGE_NAME=swiftlint-plugin
swift package plugin --list

swift package ${PACKAGE_NAME}
swift package --allow-writing-to-package-directory ${PACKAGE_NAME} --verbose

```
 
#### Build tool plugin
Build tool plugin: extend build system(bs)'s dependency graph -> generate source files, generate resources
With build plugin you can run an executable before a build(just like we do with )

unlike command plugin, with build tool plugin you can specify which targets should run each script

It creates and returns build tools invocations for Xcode to run later when the package is built
can define commands that run during the build or before the build
output file ares stored with other build artifacts, so it persists in incremental builds


## How does it work under the hood?
Each plugin runs as a separate process, has access to all of the input package, including its source files, also its dependencies, can create files and directories and perform other actions using standard libraries
A plugin runs in a sandbox and only allows writing in some places in the filesystem, like th build output directory
Also can ask for permission to write in package source directory
can output warning and errors backk to xcode

### Code
Package  plugin entry point
the same file that you define the plugin also define the main entry point of the package

## References
- [Implement Your First Swift Package Build Plugin](https://betterprogramming.pub/implement-your-first-swift-package-build-plugin-9835a7aded0b)

