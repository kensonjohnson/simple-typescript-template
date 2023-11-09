# Simple TypeScript Template

A no bells and whistles template to setup a TypeScript project. Perfect for beginners just learning TS.

## Table of Contents

- [Quick Start](#quick-start)
  - [Two Ways to Run Your TS Files](#two-ways-to-run-your-ts-files)
    - [Automatically (Recommended)](#automatically-recommended)
    - [Manually](#manually)
- [About This Repo](#about-this-repo)
- [Usage](#usage)
- [Guided TypeScript Setup](#guided-typescript-setup)
  - [Project Setup](#project-setup)
  - [Installing TypeScript](#installing-typescript)
  - [Adding Types](#adding-types)
  - [Testing the setup](#testing-the-setup)
  - [Setting Up the Dev Environment](#setting-up-the-dev-environment)
    - [ts-node](#ts-node)
    - [nodemon](#nodemon)
  - [Building For Production](#building-for-production)
- [Conclusion](#conclusion)

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

For running any TS file, just run `npm run file path/to/file.ts` to run the file. For example, if you wanted to run the `index.ts` file from the project root, you would run `npm run file src/index.ts`.

## About This Repo

This repo was made to give newcomers to TypeScript a simple environment to learn what it's all about. I will make some assumptions about your knowledge of JavaScript, but I will try to explain things as best as I can.

## Usage

Some helpful npm scripts are available in the `package.json` file:

`npm start`

`npm run build`

`npm run dev`

`npm run file`

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

Once you're sitting in your project's directory, we need to initialize this project as a Node project.
_This assumes you have Node installed on your computer. If you need to install Node, get the latest version for your OS from [the official Node.js site](https://nodejs.org/en)._
Once you have Node installed, run the following command to initialize this directory as a Node project:

```sh
npm init -y
```

This will give us a `package.json` and the start of our project!

If you want to upload your project to github, _now is a good time to create your `.gitignore` and add `node_modules` to it!_ This command will create a `.gitignore` file and add `node_modules` to it:

```sh
echo "node_modules" > .gitignore
```

We haven't initialized this directory as a `git` repo, but this will save us from messing up later, if we decide to add git to our project.

### Installing TypeScript

Let's get TypeScript installed.
TS is something that only runs in our development environment, so we can save it as a dev dependency. Notice the `--save-dev` flag:

```sh
npm install --save-dev typescript
```

This downloads TypeScript and adds it to our `node_modules`, but we also need to configure it so it knows what rules to follow.
We do this by calling the init function in the TypeScript compiler (TSC):

```sh
npx tsc --init --rootDir src --outDir build \
--esModuleInterop --resolveJsonModule --lib es6 \
--module commonjs --allowJs true --noImplicitAny true
```

_`npx` differs from `npm`: use `npm` to **install** a package and use `npx` to **execute** (run) a program._

Running the above command results in a `tsconfig.json` being created with some sensible defaults.
If you wanted to, you could look into this file to read up on the settings turned on in your project.

One thing to note here is the opinionated project structure.
The TypeScript config file is setup to look for files in a directory called `src` and it builds the final project into a directory called `build`.
You don't have to do it this way, but I would leave it as is if you're new.
I will explain why we are setting the project up this way in a later step.
Let's go ahead and create that `src` directory:

```sh
mkdir src
```

All of your `.ts` and `.js` files should go in this directory.

### Adding Types

Thanks to [this awesome npm package](https://github.com/DefinitelyTyped/DefinitelyTyped "DefinitelyTyped Project Repo"), we can simply download premade type libraries for most popular packages.
Whenever you want to get the types for a package, simply use this format during the install: `@types/name-of-package`.
We know that we're going to be using Node, so we might as well grab the types for it:

```sh
npm install --save-dev @types/node
```

_Notice that we saved our types as a dev dependency using `--save-dev`. Types are for the Development Environment._

### Testing the setup

To test our setup, we should write a little bit of TS and run the compiler.
Make an `index.ts` file in the `src` directory and add the following code:

```TypeScript
const myMessage: string = "Hello TypeScript!"

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

What we have right now is really cool, but it does get very tiring having to run `npx tsc` and manually run my JavaScript file everytime I want to see the changes.
We can add some helpful tools to help us out:

- `ts-node` - Runs TypeScript files the way that Node runs JavaScript files.
- `nodemon` - Monitors the files in you project and restarts Node each time a file is saved.

Having these will allow us to see our code changes in real time.
I'll go over each of these in detail.

#### ts-node

I'll start with `ts-node` and show how to run a TypeScript file with it.
Let's start by installing it as a dev dependency:

```sh
npm install --save-dev ts-node
```

Once again, this is a dev dependency because it's only used in the development environment.
To run a file with `ts-node`, we use the following command:

```sh
npx ts-node path/to/file.ts
```

So, if we wanted to run the `index.ts` file from the project root, we would run `npx ts-node src/index.ts`.
As mentioned above, `npx` is how we run a program that we installed with `npm`.
Running the above command will interpet out TypeScript file and run it with Node.
This is great for running a file one time and seeing the output.

Knowing how to run files manually is great, but we want to run our environment to handle running our files for us.
Let's do that by setting up a script for `npm` to run.

Open up the `package.json` file and locate the section called `scripts`.
By default, there is a `test` script in there, which we won't be using.
We can delete that and add our own script called `file`:

```json
"file": "npx ts-node src/index.ts"
```

Any key -> value pair you add here will be available to run with `npm run`.
So, if you wanted to run the `file` script, you would run `npm run file`.
Let's run our `index.ts`:

```sh
npm run file scr/index.ts
```

While not terribly useful to us right now, knowing `npm` scripts will be very useful in the next section.

#### nodemon

Instead of running our files manually each time we want to see the output, it would be awesome to start setting up our environment to do that for us. We can rerun our files each time we save them if we setup a nifty package called `nodemon`.

Think of `nodemon` as a "Node Monitor" that will allow us to re-run our code each time we save a file. I'll show you how to set it up and then explain what's going on.

First, install `nodemon` as a dev dependency:

```sh
npm install --save-dev nodemon
```

Next, create a file in the root of your project called `nodemon.json` and add the following:

```json
{
  "watch": ["src"],
  "ext": ".ts,.js",
  "ignore": [],
  "exec": "tsc && node ./build/index.js"
}
```

The above configuration file is rather simple, and we can go over each bit:

- `watch` tells nodemon which directories to look in.
- `ext` specifies what file extensions to monitor.
- `ignore` specifies files skip over and not watch.
- `exec` is the command that will be ran when a file is changed.

What you might notice is that we aren't running `ts-node` in the `exec` command.
In fact, we aren't specifying any `.ts` files at all!
What gives?
Well, it is a common pattern to compile your TypeScript into JavaScript first, and then run the plain JavaScript.
This is the most likely setup you will see when working on a project with TypeScript.

Let's break down exaclty what is happening in the `exec` command:

- `tsc` - This is the TypeScript compiler. It will compile all of our TypeScript files into JavaScript. The result will be a `build` directory with all of our JavaScript files in it.
- `node ./build/index.js` - This is the command that will be ran after the TypeScript compiler finishes. It will run the `index.js` file from inside the `build` directory.

Remember we ran `npx tsc` earlier and it created a `build` directory with an `index.js` file inside?
This is why we set up our project structure the way we did.
In most projects, you will see some type of `src` directory with all of the TypeScript files and a `build` directory with all of the JavaScript files.
Though, the specifics of the project may differ, such as having a `dist` directory instead of a `build` directory, or having a `app` directory instead of a `src` directory.

What's important to recognise is that TypeScript uses a _transpiler_, and doesn't run the code directly.
Your awesome TypeScript code is converted into JavaScript, and then the JavaScript is ran.
This is why TypeScript is considered to provide build-time type checking, and not run-time type checking.
There are no types when _running_ the code, only when _building_ the code.

With all of that being said, let's start making our dev environment.
Open up the `package.json` and locate the section called `scripts`.
Inside of scripts add the following:

```json
"dev": "npx nodemon"
```

Now you can just run `npm run dev` one time and it will start monitoring your project.
Each time you save the file, it will run the TypeScript compiler and then run `index.js` from inside your `build` directory.
Nice!

When you want to stop the dev server, press `CTRL + c` on your keyboard. This isn't specific to `nodemon`, but is a common way to stop a process in the terminal.

### Building For Production

I hope I've made it abundantly clear that TypeScript is not ran in the production environment.
It is only used in the development environment to help us write better "type safe" code.
When we are ready to deploy our code, we need to build it into JavaScript.

While not important for what we're doing in this project, I want to simulate what "building for production" looks like.

Let's start off by adding a couple more scripts to our `package.json`:

```json
"build": "tsc",
"start": "node ./build/index.js"
```

The `build` script will run the TypeScript compiler and build our project into JavaScript.
The `start` script will run the `index.js` file from inside the `build` directory.

Nothing about this is helping our project, but most frameworks will optimize the build during this step.
This could mean "minifying" the code (removing all whitespace and comments), or "tree shaking" (removing unused code), or "bundling" (combining all of the code into a single file).
What results is a smaller and more optimized file that is ready to be deployed.

We can turn on some of the optimizations ourselves by adding another flag to the `tsconfig.json` file.
Open up the `tsconfig.json` find the line that says:

```json
// "removeComments": true, /* Disable emitting comments. */
```

and change it to:

```json
"removeComments": true, /* Disable emitting comments. */
```

Go back to your `index.ts` file and add a comment:

```TypeScript
// This comment is only for the source code
const myMessage: string = "Hello TypeScript!"

console.log(myMessage)
```

Now run `npm run build` and open up the `index.js` file from inside the `build` directory.

Notice that the comment is gone:

```JavaScript
"use strict";
const myMessage = "Hello TypeScript!";
console.log(myMessage);
```

This is a very simple example of what happens when a framework builds your code for production.
It's not useful here because it would be better to keep the comments in while you're learning TypeScript.
I suggest changing the `removeComments` flag back to `false` in the `tsconfig.json` file for now.

## Conclusion

I hope this repo has helped you get started with TypeScript.
It may seem overwhelming at first, but it's really not that bad. I suggest you start by writing some simple JavaScript in `.ts` files, which gives you many benefits from "infered types" and code completion. Start adding types to your variables, then to your functions, and eventually you will get the full benefits of TypeScript.
