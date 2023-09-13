---
layout: post
title: "Ordering elements in a PDF, and dealing with an ATS"
date: 2023-09-12 15:30
tags: design
navigation: true
class: post-template
current: post
---

We're stepping away from the world of programming for a moment to talk about a design-related topic.

I've recently been working on my resume and have been learning a lot of new things.

The last time I was in school I took a career class, with a few days dedicated to resumes. They showed us all the latest trends in resume designs, and I thought I came up with something fairly eye-catching. If I'm honest, it was just a template from Canva that I adjusted a bit, so it wasn't my original design, but I found the software very easy to work with. I liked the drag-and-drop functionality and the end result. It produced a nice PDF with a two-column layout.

I ended up getting my next two jobs through my school or people I knew, so this resume worked fine. I have been updating the same resume with new skills and work experience ever since, up until recently.

I happened to bring this up with a friend that had her resume reviewed by the career services at her university, so I asked her to have a look at mine. One of her main criticisms was the two column layout. If I were to use it to apply to jobs online, applicant tracking systems may not be able to read it, because these systems read documents left to right. I looked into this a little more and this seems to be the consensus about two-column layouts, although it's possible some systems are sophisticated enough to read them anyway.

Assuming most systems aren't at that level of sophistication, though, it means the text from the left and right column will be jumbled together. Of course the systems wouldn't be able to parse this.

### First attempt at fixing it - creating a one-column layout

I used Canva again to make a one-column resume. I ran it through a resume checker to see if it scored well, and it did not. It couldn't find important sections of my resume that I had included. I was very confused as to why, since now it should be in a more readable format, right?

### Second attempt - creating the template myself in Illustrator

I thought maybe Canva was doing something weird with the formatting, so I decided to create a new resume from scratch using Adobe Illustrator, creating every element from scratch myself.

I ran the new document through the resume checker, and found a similar result. Some parts of my resume were coming through as expected, like my contact information, but some sections were still missing. I googled the problem, and some of the solutions mentioned the order the elements are created in the document can cause things to appear out of order, and to check what happens when you copy the text from the PDF into Notepad.

When I copied the text into Notepad, it was a mess. Everything was completely out of order, even though it appeared normal in the PDF. Text didn't properly follow the headings, which is why the resume checker couldn't locate it.

### Third attempt - re-ordering the elements before export

The way I chose to fix this was by going into the layers panel in Illustrator and manually putting the elements in the correct order. I could see that the order they were in before this fix matched the way they showed up in Notepad, so this would likely fix my problem.

After exporting the new document, I pasted the text into Notepad again, and this time the order was correct.

### Conclusion

Despite not being a programming problem, this issue felt a lot like the normal debugging I do day-to-day. I find it so frustrating that these problems you cannot easily see could completely throw off someone's job search, and they may not even realize it.

There is a lot of advice given about resumes, and it seems to change constantly. This is a whole separate skill set than a developer skillset. It would be easy to feel like these automated rejections are rejecting your developer skillset and experience, or even you as a person. But it's truly not that personal. The entities reading this curated summary of your work life aren't even human. All I can say is keep learning new skills and trying your best.
