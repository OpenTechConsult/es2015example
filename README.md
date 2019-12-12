# FRONT END BUILD SYSTEM

Hi! This repository is a sandbox for learning how to use [node.js](https://nodejs.org/en/) as front-end build system. Node.js is no more only use by server-side or back-end JavaScript developer. Node.js is nowadays used quite a lot by front-end developers.

- NodeJS is use for generating front-end module such as React, or lodash (which can be use on the client side or server side). 

- Node.js is also use by front-end developer to build portable backward compatible JavaScript code. Example __Transpilers__ such as [Babel](https://babeljs.io/) are used to convert modern ES2015 JavaScript code into a more widely supported ES5 JS code. Other tools inclide minifiers [UglifyJS](https://github.com/mishoo/UglifyJS)and linters [ESlint](http://eslint.org) for verifying the correctness of the code.
- Test runners are now also driven by Node.

When we mix and juggling all these tool we need a way to record how the build process work. To that end, some projects use npm scripts, others use [Gulp](https://gulpjs.com/), and some others use [Webpack](https://webpack.js.org/)

In this sandbox, we are going to use NPM to run Script and specifically to create custom npm scripts

## Using npm feature to run script

npm has built-in features to run scripts. Npm has pre-define command that allows us to do specific tasks such as

npm start : to start a web app or electron app
npm stop : to stop a web server
npm install : to run native build commands

command are added to the script property of the package.json file 

example:

```json
{
    ...
    "scripts": {
        "start": "node server.js"
    },
    ...
}
```

But a lot of tasks that we may want to define will not fit into the npm predefined scripts commands. For example if we want to transpile a file from ES2015 to ES we cannot do so. To achieve that we need to use `npm run` command to define custom npm scripts

### Creating custom npm scripts

The **`npm run`** command alias of npm _run-script_ is used to define arbitrary scripts invoked with :
> npm run script-name

Let see how to make on building client side-script with Babel.

We start by setting up this new Node project and install the necessary dependencies

> npm init -y
> npm install --save-dev babel-cli babel-preset-es2015
> echo { "presets": ["es2015"] } > .babelrc

#### include the custom 'babel'  script

Next we insert the custom `babel` property under the _script_ in package.json file . This will run the script that has been installed in the ./node_module/bin folder

We create a file called browser.js in the ES2015 syntax.

### Go deep into npm script customization

If all our project custom script does is to transpile the browser.js file in to ES5, we can change the name of **babel** to **build** in the package.json
But let's go further by installing UglifyJS as well. Let's install UglifyJS :

> npm i --save-dev uglify-es

Uglifly can be invoked by using `node_modules/.bin/uglifyjs` so we need to add it to the package.json under _scripts_ with the name uglify like this

> ./node_modules/.bin/uglifyjs build/browser.js -o build/browser.min.js

We should now be able to invoke `npm run uglify`

But we can tie all of this together by combining both of these scripts. To do that, let's add another script property called **`build`** that invokes both tasks

> "build": "npm run babel && npm run uglify"