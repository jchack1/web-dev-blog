---
layout: post
title: "Helpful design notes - fonts"
date: 2021-03-02 15:30
tags: design
navigation: true
class: post-template
current: post
---

It's been a while since we had a new post on design so here we go.

Today I'm back working on my UX Design / Adobe XD course so I thought I'd make some notes here with some helpful tips. Today is mostly about fonts.

<strong>fonts.google.com</strong> - free fonts

<strong>fonts.adobe.com</strong> - paid fonts, should be included in creative cloud subscription

For Adobe fonts, can click "activate font" and use it in Adobe XD.

For Google fonts, to use in your design software, you need to download the font and install on your machine.

### Choosing fonts

Decorative fonts are good for headers, but not great for body text. If you make your text more narrow, you can fit more characters in your titles. It doesn't look as good if your titles break onto multiple lines.

Fonts can slow down website loading. Your browser has to go find the font and download it for use on your site. If you have many fonts with many different weights it takes longer for your site to load. Decreasing the number of fonts and weights you include in your site can help with loading time and also with SEO - Google finds these sites to be more friendly. Google fonts will let you know if your load time is fast, moderate, slow, and so on.

Search for font combinations if you are unsure what fonts you should pair together.

### Common font sizes for your designs

- Heading 1 - 40
- Heading 2 - 32
- Heading 3 - 24
- Body copy - 16

These work well with your baseline grid in XD, if it is also divisible by 8. Items line up nicely with your grid this way.

In XD, add your fonts into Character Styles so you have your fonts quickly available for you and your team throughout the design process. This is helpful if you need to change your font - XD will find all instances of this font being used and update it when you change the font.

While doing a bit of my own research in this area I came across [this article on learnui.design](https://learnui.design/blog/mobile-desktop-website-font-size-guidelines.html).
Their recommendation was to start with a font size of 16px and go from there, depending on your situation.

If your site is text heavy, a larger font is more desirable, as it is easier to read and helps the user avoid eye strain.

If the site is interaction heavy, an app like Facebook maybe, then you wouldn't want to go higher than 16px. Users are more likely to be scanning the page than reading closely, so larger text doesn't feel quite right.

Finally, they recommended to not have more than 4 font sizes. This looks a little different than what's shown above.

1. A header size - add another smaller size if you need a sub-heading
2. Your default size - 16px
3. Secondary size - a couple points smaller than your default, eg. 14px, for less important details and captions
4. Tertiary size - in case you need another smaller size, not always necessary though, again things like labels and captions

### Buttons

Looking at design systems like Google Material can help you get inspiration to make better buttons. They add things like rounded corners, drop shadow, etc, to make buttons look more clickable.

You can add icons like arrows, plus, chevrons - to show convey that the button performs some actions.

Colours are important for buttons to convey the correct meaning. Important buttons, like delete, could be red. Be consistent with your colours across your site so users don't get confused.

A good site to visit to learn more about UX/UI concepts is [UXPlanet.org](https://uxplanet.org/).

[This article](https://uxplanet.org/primary-secondary-action-buttons-c16df9b36150) has some good tips for creating primary and secondary buttons, including:

- <strong>making buttons distinct</strong> - could use a brighter colour for the primary button, for example. But be careful about buttons that cause a negative action, like delete buttons. Make these less obvious than the accompanying "cancel" button. You don't want users to take a permanent action by mistake because they just clicked on the most obvious button.
- <strong>button labels should explain what they do</strong> - describe the action with a verb, for example use "submit" instead of "ok"
