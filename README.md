# simple-typescript-template

A no bells and whistles template to setup a TypeScript project. Perfect for beginners just learning TS.

## Quick Start

Click on the "Use this template" button located above.
From that dropdown, select "Create a new repository" and give your new repo whatever name you want.

Once your new repo is created, clone it down and run `npm install` from within the project directory.

Running a TS file is similar to running a JS file with `Node`, with a minor difference:

### Two Ways to Run Your TS Files

#### Automatically (Recommended)

To run your code automatically on save, run `npm run dev`.

This will run any code in the `index.ts` file located in the `src` directory.

_When you first start learning TypeScript, this allows you to simply write your code in that `index.ts` and see the results each time you save the file._

#### Manually

For running any TS file, just run `npx ts-node path/to/file.ts` to run the file.

## About This Repo

This repo was made to give newcomers to TypeScript a simple environment to learn what it's all about.

## Usage

Some helpful npm scripts are available:

`npm start`

`npm run dev`

`npm run test`

`npm run build`

## Guided TypeScript Setup

Want to know the steps it takes to setup a project like this?
I'll break it down right here.

### Project Setup

The first thing you need is to initialize your project.
Make a folder and give it the name of your project:

```sh
mkdir my-project
cd my-project
```

Once you're sitting in your project's directory, we need to initialize this project as a Node project:

_This assumes you have Node installed_

```
npm init -y
```

This will give us a `package.json` and the start of our project.
_Now is a good time to create your `.gitignore` and add `node_modules` to it!_

### Installing TypeScript

Let's get TypeScript installed.
TS is something that only runs in our development environment, so we can save it as a dev dependency:

```sh
npm install --save-dev typescript
```

This downloads TypeScript, but we also need to configure it so it knows what rules to follow.
We do this by calling the init function in the TypeScript compiler (TSC):

```sh
npx tsc --init --rootDir src --outDir build \
--esModuleInterop --resolveJsonModule --lib es6 \
--module commonjs --allowJs true --noImplicitAny true
```

_`npx` differs from `npm`: use `npm` to **install** a package and use `npx` to **execute** (run) a program._

Running that command results in a `tsconfig.json` being created with some sensible defaults.
If you wanted to, you could look into this file to read up on the settings turned on in your project.

One thing to note here is the opinionated project structure.
The TypeScript config file is setup to look for files in a directory called `src` and it builds the final project into a directory called `build`.
You don't have to do it this way, but I would leave it as is if you're new.
Let's go ahead and create that `src` directory:

```sh
mkdir src
```

All of your `.ts` and `.js` files should go in this directory.

### Adding Types

Thanks to a [really awesome package](https://github.com/DefinitelyTyped/DefinitelyTyped "DefinitelyTyped Project Repo"), we can simply download premade type libraries for most popular packages.
Whenever you want to get the types for a package, simply use this format during the install: `@types/name-of-package`.
We know that we're going to be using Node, so we might as well grab the types for it:

```sh
npm install --save-dev @types/node
```

_Notice that we saved our types as a dev dependency using `--save-dev`._

### Testing the setup

To test our setup, we should write a little bit of TS and run the compiler.
Make an `index.ts` file in the `src` directory and add the following code:

```TypeScript
const myMessage: String = "Hello TypeScript!"

console.log(myMessage)
```

And run the compiler with:

```sh
npx tsc
```

You will notice a new directory called `build` with a file called `index.js` inside.
Opening up this file, you should see this:

```JavaScript
"use strict";
const myMessage = "Hello TypeScript!";
console.log(myMessage);
```

This is the TypeScript converted into valid JavaScript!
You can now create your project inside of the `src` directory and compile to JavaScript whenever you want.

### Setting Up the Dev Environment

What we have right now is really cool, but it does get very tiring having to run `npx tsc` everytime I want to run my code.
We can add some helpful tools to help us out:

- ts-node - Runs TypeScript files the way that Node runs JavaScript files.
- nodemon - monitors the files in you project and restarts Node each time a file is saved.

Having these will allow us to see our code changes in real time.
These are dev dependencies like all the rest we've talked about above, so install like this:

```sh
npm install --save-dev ts-node nodemon
```

Next, create a file in the root of your project called `nodemon.json` and add the following:

```json
{
  "watch": ["src"],
  "ext": ".ts,.js",
  "ignore": [],
  "exec": "npx ts-node ./src/index.ts"
}
```

The above configuration file is rather simple:

- `watch` tells nodemon which directories to look in.
- `ext` specifies what file extensions to monitor.
- `ignore` specifies files not to look at.
- `exec` is the command that will be ran when a file is changed.

Now we can make a convenience script for running our dev environment.
Open up the `package.json` and locate the section called `scripts`.
Inside of scripts add the following:

```json
"dev": "npx nodemon"
```

Now you can just run `npm run dev` one time and it will start monitoring your project.
Each time you save the file, it will run the TypeScript compiler and then run `index.js` from inside your `build` directory.
Nice!
