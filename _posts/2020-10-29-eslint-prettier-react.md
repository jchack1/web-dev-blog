---
layout: post
title: "How I configured ESLint and Prettier for React at work "
date: 2020-10-29 17:30
tags: react devops
navigation: true
class: post-template
current: post
---

At work we are doing some set-up on our projects so we can work better as a team, especially as we grow. It made sense to look into linting and formatting we can use across our projects, so our code is consistent and we don't have to spend time thinking about correct formatting as we code.

We decided we wanted to implement ESLint for linting (looking for code quality issues) and Prettier for formatting (code format/style like quotes, tabs, etc). We specifically wanted to add config files to the projects where we can store our settings, so the settings are just there in our project and we don't have to change too many settings in our code editor.

For this set-up you need to install the packages locally in your project, and install the extensions directly in your editor updating a couple settings.

Since we use React I made an example project of how you set up ESLint and Prettier from a brand new React project. The steps I followed are below, as well as some other info, some of which is copied from my work document.

### Steps

1. Create folder for new app and open in VS Code
2. Open terminal and run command `npx create-react-app . ` to create app directly in your folder
3. Go to “Extensions” tab and install “ESLint” and “Prettier - Code formatter” in VS Code
4. Install packages locally; run this command in the terminal: `npm i --save-dev eslint prettier eslint-config-prettier eslint-plugin-prettier`
   - Note: ESLint React plugins come pre-installed with create-react-app
5. Add Prettier config file
   - “.prettierrc”
   - Inside the file add a JSON object, your Prettier rules will go here
6. Add ESLint config file
   - “.eslintrc.json”
   - Add empty JSON object
   - Add “extends” section with the following: `“extends”: [“eslint:recommended”, “react-app”]`
     - “React-app” extends the React rules that are already included with create-react-app
7. Go to File -> Preferences -> Settings ( or Ctrl + , ) to update settings for Prettier extension
   - Text Editor -> Formatting -> check “Format on Save”
   - Ensure “Format on Save Mode” is set to “File”
   - In the search bar search for“default formatter” and in the dropdown select “esbenp.prettier-vscode”
     - This allows prettier to format all our file types

### ESLint can extend Prettier rules

You can set up ESLint to give you warnings when your Prettier options are not being followed. We decided not to go with this for now, because we found that they were quite distracting when working in your editor. However, this is what you would need in your .eslintrc.json file if you wanted that:

`"extends": ["eslint:recommended", "react-app" / "prettier"], "plugins": ["prettier"],`

### Husky for git

We can use Husky to lint our code before we commit our code with git.

Install Husky with the following command: `npm i husky --save-dev`

In our package.json, we want to have this in our scripts, so we can run it in the command line:
`"scripts": { "lint": "eslint ." }`

And include this Husky config as well:
`"husky": { "hooks": { "pre-push": "npm run lint" } }`

### Links

[Prettier Docs](https://prettier.io/docs/en/install.html)

[ESLint Docs](https://eslint.org/docs/user-guide/getting-started)

[Helpful youtube tutorial](https://www.youtube.com/watch?v=bfyI9yl3qfE) - NOTE I did not follow this exactly, but it helped me to learn how ESLint and Prettier can be set up with create-react-app
