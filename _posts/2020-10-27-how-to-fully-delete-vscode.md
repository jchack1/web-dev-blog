---
layout: post
title:  "How to fully delete Visual Studio Code from Windows 10"
date:   2020-10-27 14:30
tags: devops
navigation: true
class: post-template
current: post
---

Recently I was messing around in my VS Code settings trying to get some new extensions to work. Long story short, I was following along with a tutorial, clicked a few things I probably shouldn't have, and suddenly I could not see the settings UI screen.  

I figured it would be best to just uninstall VS Code and start over fresh, so I went to "Add or remove programs" in system settings to uninstall the program. Seemed simple enough, but when I re-installed VS Code, all my extensions were still there, and my settings were still not right.  So VS Code must be saving data elsewhere on the machine and it is not being deleted when the program is uninstalled. 

I spent some time navigating my PC's files to determine where the program files were kept, but all it really took was a quick Google search to find some helpful answers.

To be extra sure, as I found myself confused for the first time about the folder stucture of my PC, I went over a couple different pages:
- one on [Stack Overflow](https://stackoverflow.com/questions/47689536/uninstall-visual-studio-code-in-windows)
- another on [Stack Exchange](https://superuser.com/questions/1380208/how-to-completely-uninstall-visual-studio-code-from-windows-10)

I'll list what I did below. It may be overkill, but when I reinstalled VS Code afterwards I had a clean slate; no more extensions and default settings.


### What I did

1. Go to ``` C:\Program Files\Microsoft VS Code```
2. Click ```unins000.exe``` and uninstall VS Code 
3. Go to ``` %AppData%\Code ``` and delete the Code folder
4. Go to ``` %UserProfile%\.vscode ``` and delete .vscode folder
5. Go to ``` %AppData%\Local\Programs\Microsoft VS Code``` and delete this folder 


Basically delete any files having to do with VS Code, and it should get rid of any VS Code data lurking around on your machine.