---
layout: post
title: "Usability - notes from design course"
date: 2020-09-07 14:30
tags: design
navigation: true
class: post-template
current: post
---

I am back working on my Udemy course, "Master Digital Product Design: UX Research and UI Design". In this post I will add some notes on usability.

Usability Heuristics, I have learned, is a way of saying "rule of thumb". So these are not usability rules exactly; they are usability suggestions and best practices.

While going through these lectures I have been cringing a bit. Recently I realized I am becoming more interested in and passionate about good user experience and easing frustration for the user. The designer in me knows how important these points are. However, I know I have broken some of these best practices as a developer. It has been difficult for me to implement my features in the best possible way when task requirements are unclear, on large systems I did not create myself. I came across roadblocks and had to get around them somehow, and then I was irritated with myself for not implementing the feature in the "best" way. But in those sitations, I did what I could.

Nevertheless, I'm still enjoying learning about UX and UI.

## Useful links

This site [UI-patterns.com](https://ui-patterns.com/patterns) has in-depth descriptions and examples of many design patterns you see in user interface design, including cards, modals, dropdown menus, and more.

This [contrast checker](https://contrastchecker.com/) site allows you to check the foreground and background colors on your site against various conditions, to make sure the level of contrast is acceptable. You could use this while auditing your sites.

## Usability Heuristics

<strong>Visibility: </strong> (of system status) includes the idea of instant feedback - does a button instantly change when clicked? Is there a loading bar/spinner when a file is downloading?

<strong>System and real world matching: </strong> does the language used sound like how we speak in real life? Or in the industry the software is used in?

<strong>User control and freedom: </strong> can the user easily escape their previous action? Nothing we build is fully intuitive and the user will make mistakes. Can you reset, undo, go back? This allows users to explore and experiment, since they won't be afraid of clicking and making irreversible mistakes.

<strong>Consistency and standards: </strong> does it use existing conventions and standards used on other sites? Does it follow design patterns we typically see on websites? Some examples are hamburger icons for menus, paper clip icons for attachments, and shopping cart icons on e-commerce sites. Sites can also create their own standards - for example, Google uses different colored buttons for different functions.

<strong>Error prevention: </strong> warn users if they are going to perform a significant action, like clearing a form. Or prevent an action altogether.

<strong>Recognition rather than recall: </strong> storing important information on the screen rather than making the user remember. For example, highlighting what page they're on or category they are in so they don't have to remember where they are in the site.

<strong>Flexibility and efficiency of use: </strong> making the software easier to use for advanced users, so they can quickly finish routine tasks.

<strong>Aesthetics and minimalism: </strong> you want the minimal amount of things on the screen, so we don't overload the user's working memory.

<strong>Help recognize, diagnose, and recover from errors: </strong> do we tell users about the errors clearly, and can they easily find the solutions?

<strong>Help and documentation: </strong> is there help available and can users find where it is? Are the steps clear?

## Usability audits

These are different from usability heuristics. Heuristics are vague, but an audit is more of a checklist. These are rules you come up with as part of your design process. Creating the audit can be time-consuming, but filling it out should be relatively quick and simple. Audits should contain specific yes or no questions that anyone can fill in while reviewing the product. No opinions should be allowed.

Example questions on the checklist (from a real checklist shown in the course):

- Does every display begin with a title or header that describes screen contents?
- Is there visual feeback in menus or dialog boxes about which choice the cursor is on now?
- Is the current status of an icon clearly indicated?

## Testing

It is often recommended that testing be done often, and that everyone in your company should observe users using the software regularly. Programmers, managers, everyone else; everyone involved with the product should observe the software being used.

Don't just ask users what they like. They will start to post-rationalize reasons why they like or dislike it, and start coming up with their own solutions. They will think about what would be useful in a hypothetical situation. But that's not actually very useful for us. It's important to <em>observe</em> them using it.

A <strong>usability test</strong> is when you give a user a task and observe what they do. You look for responses in their faces, and you need to be impartial and not influence the user's behavior. Get users to talk out their thought process while using the product - that way you don't have to guess.

Reasons to do usability testing with experienced users as well as new users:

- experienced users may not even be experts in the software; users often fiddle their way through until they complete what they need to do, without mastering the product
- products tested with only first time users can sometimes be set up really well to guide users through set up and early stages, but tasks they attempt later on may be very frustrating because they haven't been given as much attention
- existing customers are very valuable, more so than new customers

Test as early as you can! Don't wait until you have a finished design.

<strong>Click testing: </strong> a quantitative method for testing, where users are given a task and you can see where they click in your design

- you can test a large number of people and get data about how they are navigating your product
- however, you just don't know how they were feeling while they were going the test, so it is hard to gauge the reason they clicked where they did, and how big of a problem it is when a user does something "incorrectly"
- a site you can use for this is [usabilityhub.com](https://usabilityhub.com/)
