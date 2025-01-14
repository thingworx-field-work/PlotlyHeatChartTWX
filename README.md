# Starter kit of a widget using webpack and typescript

This repository contains an example that can act as a starter kit for building widget using modern web development techniques like [webpack](https://webpack.js.org/) and [typescript](https://www.typescriptlang.org/) and [babel](https://babeljs.io/).

## Why use it

There are many advantages to this, and here's some of them:

### Webpack advantages

* Because of *dynamic imports and require calls*, webpack can load javascript files and other resources only they are needed. So, if you are using a very big external library, this library is only loaded if the widget is used in a mashup, rather than being always loaded in the `CombinedExtensions.js` files.
* *Better resource management*: You can load a resource (like images, xml files, css) only if it's used in the widget, and you'll no longer have to care about where to put it and how to package it. Also, the webpack loader will inline small files for better network performance.
* *Automatic dependency management*: Webpack can be very easily integrated with the `npm` repositories, so this brings automatic dependency management. You'll no longer have to manually download libraries from the web, struggle to add all the dependencies and so on. Instead, `npm`, which functions similarly to maven central, handles all of this.
* *Easily develop and test the widget outside of thingworx*: By doing the initial development and testing in a simple html page, it reduces the waiting times of publishing widget, doing reloads, etc...
* *Allows using of different loaders*: You can develop your code in multiple languages, and use transpilers to convert you code javascript code that works in older javascript version. One of this languages, is typescript.

### Typescript

* Typescript is a superscript of javascript with types.
* Optional static typing (the key here is optional)
* Type Inference, which gives some of the benefits of types, without actually using them
* Access to ES6 and ES7 features, before they become supported by major browsers
* The ability to compile down to a version of JavaScript that runs on all browsers
* Great tooling support with IntelliSense

## Using the widget template

### Required software

The following software is required:

* [NodeJS](https://nodejs.org/en/) needs to be installed and added to the `PATH`. You should use the LTS version.

The following software is recommended:

* [Visual Studio Code](https://code.visualstudio.com/): An integrated developer enviroment with great typescript support. You can also use any IDE of your liking, it just that most of the testing was done using VSCode.

### Proposed folder structure

```
demoWebpackTypescriptWidget
│   README.md         // this file
│   package.json      // here you specify project name, homepage and dependencies. This is the only file you should edit to start a new project
│   tsconfig.json     // configuration for the typescript compiler
│   webpack.config.js // configuration for webpack. Can be updated through the use of webpack-merge
│   index.html        // when testing the widget outside of thingworx, the index file used.
│   .env.sample       // sample file of how to declare the target ThingWorx server for automatic widget upload. Rename this file to .env
│   .eslintrc.js      // eslint configuration for automatic file formatting
│   .releaserc.json   // semantic-release sample configuration for publishing to GitHub Releases
│   .releaserc-gitlab.json   // semantic-release sample configuration for publishing to GitLab Releases
└───webpack           // Internal webpack configuration and plugins
└───Entities          // Thingworx XML entities that are part of the widget. This can be Things, StyleDefinitions, etc. They can be exported using the SourceControl export functionality in Thingworx.
└───src               // main folder where your developement will take place
│   │   index.ts               // source file used when testing the widget outside of twx
│   │   demoWebpack.ide.ts     // source file for the Composer section of the widget
│   │   demoWebpack.runtime.ts // source file for the Runtime section of the widget
│   └───internalLogic          // usually, put the enternal logic into a separate namespace
│   │   │   file1.ts           // typescript file with internal logic
│   │   │   file2.js           // javascript file in ES2015 with module
│   │   │   ...
│   └───styles        // folder for css styles that you can import into your app using require statements
│   └───images        // folder for image resources you are statically including using require statements
│   └───static        // folder for resources that are copied over to the development extension. Think of folder of images that you reference only dynamically
└───build         // temporary folder used during compilation
└───zip               // location of the built extension
```

### Developing a new widget

In order to start developing a new widget using this template you need to do the following:

1. Clone this repository
    ```
    git clone http://roicentersvn.ptcnet.ptc.com/placatus/DemoWebpackWidget.git
    ```
2. Open `package.json` and configure the `name`, `description`, and other fields you find relevant
3. Run `yarn install`. This will install the development dependencies for this project.
4. Run `yarn run init`. This will create sample runtime and ide typescript files using the name.
5. Start working on your widget.

### Adding dependencies

Dependencies can be added from [npm](https://www.npmjs.com/), using the `yarn install DEPENDENCY_NAME --save` command, or by adding them directly to `package.json`, under `dependencies`. After adding them to `package.json`, you should run `yarn install`.
If you are using a javascript library that also has typescript mappings you can install those using `yarn install --save @types/DEPENDENCY_NAME`.

### Building and publishing

The following commands allow you to build and compile your widget:

* `yarn run build`: builds the production version of the widget. Creates a new extension zip file under the `zip` folder. The production version is optimized for sharing and using in production environments.
* `yarn run upload`: creates a build, and uploads the extension zip to the thingworx server configured in `.env`. The build is created for development, with source-maps enabled.
* `yarn run buidDev`: builds the development version of the widget. Creates a new extension zip file under the `zip` folder.The build is created for development, with source-maps enabled.

## Example of widgets that use this starter kit

* [SVGViewer](https://github.com/ptc-iot-sharing/SvgViewerWidgetTWX): Allows viewing, interacting and manipulating SVG files in Thingworx. Contains examples of using external libraries.
* [Calendar](https://github.com/ptc-iot-sharing/CalendarWidgetTWX): A calendar widget for Thingworx built using the fullcalendar library.  Contains examples of using external libraries as well as referencing global external libraries without including them in the built package.

## Sematic release

The widget uses [semantic-release](https://semantic-release.gitbook.io/) and GitLab CI/CD pipelines for automatic version management and package publishing. This automates the whole widget release workflow including: determining the next version number, generating the release notes, updating the _CHANGELOG.MD_ and publishing a release. Please read through the *semantic-release* official documentation to better understand how it works.

Because we are using *semantic-release* the commit messages must follow a specific format called [Angular commit conventions or Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/). It is mandatory that this is followed. To help with this, the project is also setup with [commitizen](https://commitizen.github.io/cz-cli/) as a dev-dependency. So, you can use `git cz` instead of `git commit` to create a new commit.

The repository should have one protected branch:

* **master**: This is where the main development takes place. Each push to master is automatically built by the CI/CD pipeline and artifacts are generated.

By default, this repository comes with two samples CI/CD configurations for both GitHub and GitLab. By default, the `.releaserc.json` triggers a GitHub release. If you want to use GitLab, rename `.releaserc-gitlab.json` to `.releaserc.json`.

#This Extension is provided as-is and without warranty or support. It is not part of the PTC product suite.#


# Disclaimer
By downloading this software, the user acknowledges that it is unsupported, not reviewed for security purposes, and that the user assumes all risk for running it.

Users accept all risk whatsoever regarding the security of the code they download.

This software is not an official PTC product and is not officially supported by PTC.

PTC is not responsible for any maintenance for this software.

PTC will not accept technical support cases logged related to this Software.

This source code is offered freely and AS IS without any warranty.

The author of this code cannot be held accountable for the well-functioning of it.

The author shared the code that worked at a specific moment in time using specific versions of PTC products at that time, without the intention to make the code compliant with past, current or future versions of those PTC products.

The author has not committed to maintain this code and he may not be bound to maintain or fix it.


# License
I accept the MIT License (https://opensource.org/licenses/MIT) and agree that any software downloaded/utilized will be in compliance with that Agreement. However, despite anything to the contrary in the License Agreement, I agree as follows:

I acknowledge that I am not entitled to support assistance with respect to the software, and PTC will have no obligation to maintain the software or provide bug fixes or security patches or new releases.

The software is provided “As Is” and with no warranty, indemnitees or guarantees whatsoever, and PTC will have no liability whatsoever with respect to the software, including with respect to any intellectual property infringement claims or security incidents or data loss.
