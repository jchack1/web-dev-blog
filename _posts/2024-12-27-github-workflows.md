---
layout: post
title: "Learning Git Workflows"
date: 2024-12-27 16:30
tags: git version-control
navigation: true
class: post-template
current: post
---

In order to get a better idea of how to manage our branches in github at work, we took a LinkedIn Learning course on git workflows. Here are some notes I took during the course.

<strong>Git workflow:</strong> an agreement amongst a dev team that defines how source code is managed

There are already many established workflows you can pick from.

The main building blocks of a workflow include:

- a shared repo
- your local repo
- branches

### Types of branches

- Long-lived branches - remain open throughout the project
  - main: holds latest stable code, often holds latest code that has been released to prod
  - develop: branched off of main branch, used to integrate the next version of the code they plan to release, where team merges their work in progress
- Short-lived branches: exist for a short time, then merged
  - feature: for developing new feature
  - hotfix: used to fix critical issue, branched from main branch

### Git workflow examples

Selecting the right git workflow for your needs depends on:

- size of team
- release cadence - how frequently do you release?
- how much automation is involved in your process? CICD pipelines etc

#### Git flow

- less modern, doesn't do well with continuous delivery
- good for less frequent releases, like monthly, quarterly, etc

Long lived branches:

- main - production ready and can be released
- develop - all the development changes that are being worked on for the next release
  - where devs add their changes and pull the changes of their teammates

Short lived branches

- feature: branch from develop branch, for a single feature or bug, merged back into develop
- release: branch off of develop branch
  - if you notice issues, patch features in the release branch
  - when ready, merge into the main branch, as well as develop, so develop has any patches that were made
- hotfix: patch a production release, branch off main, apply fix, then merge it back into main and develop

Can apply branch protection rules in github settings

- example: require pull request before merging, require approvals

Some helpful commands:

- to set up a branch to track a remote branch, is: `git checkout -b develop origin/develop`
- to push new branch, and set up tracking between local repo and shared repo, try adding -u flag: `git push origin feature/jh-123-site-content -u`

Releasing to production

- merge release branch into main
- this can allow devs to keep working on the next release while the current release is getting ready
- once this is done, need to merge the changes we made to release, into development, so each branch has everything
- all you have to do is merge release into develop

Hotfix

- if you need to fix something that has already been released to main, do a hotfix
- branch new 'hotfix' branch off main
- merge hotfix branch into main
- then merge into develop so both have latest fix

#### Github workflow

- created by GitHub
- main is only long-lived branch
- no release or hotfix branches
- high release cadence, it's fine since you're releasing maybe every few hours
- branch off main locally, commit changes as they make progress, then push to remote feature branch, so they on't lose their work
- pull request into main
- same thing for hotfixes - branch off main, merge back in

Extra safeguards you should have so github workflow is successful:

- continuous integration
- automated build and test during deployment
- unit and integration tests
- peer review
- need approvers on pull requests
- automated deployment

#### Trunk-based development

- centers around single branch trunk(main)
- no long-lived branches other than main

Small teams with high release cadence:

- clone trunk locally, commit against trunk branch, then merge directly into shared repo

For large teams:

- use short lived feature branches, like GitHub flow

Release strategies:

- release directly from trunk - good for rapid pace
- create release branch before each release, then devs can work directly off trunk without impacting release
- hotfix: cherry-pick a single commit to merge into release branch
- CICD is important, like in GitHub flow

##### Feature flags

- toggle availability of feature in your software, or under specific conditions
- you could toggle a feature on for dev environemnts, but not prod, so devs can test it out

Deployment vs release

- deployment pushes code into prod, but not available yet
- release - it's now available for the user

Strategies:

- basic toggle
- targeted - to certain user segments
- rollout - increase % of users gradually
- can be added with simple if statements, or with a library
