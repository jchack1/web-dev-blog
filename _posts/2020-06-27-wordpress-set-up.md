---
layout: post
title:  "Using WordPress again - getting started"
date:   2020-06-27 12:30
tags: wordpress
navigation: true
class: post-template
current: post
---

We learned WordPress in school briefly as part of our CMS course. I could possibly get to work on some small freelance WordPress projects in the near future so I am relearning WordPress again with the site builder Oxygen.

### Setting up Wordpress and installing a new site

I already had xampp installed from school.  To run a Wordpress site locally you need a few things installed, including a server and a database.  xampp has those all packaged together when you install it.

There are lots of articles online with instructions for this, but I followed my class notes - they are currently located [here on my instructor's github](https://github.com/acidtone/wbdv-fall-2019/tree/master/cpnt200/chapters/ch01). Hopefully this repo will be around for a while but I will also explain the process here.

#### Steps

1. Go to [apachefriends.org](https://www.apachefriends.org/download.html) and download xampp.
2. Open the xampp control panel - I have a shortcut on my desktop to the control panel to make it easier to locate.
3. Click "Start" for both Apache and MySQL
4. Click "Admin" next to MySQL. This opens up localhost/phpmyadmin/ where you can create a database
5. Click "New", input the name you would like for your database, and click "Create"
6. Download WordPress from [wordpress.org](https://wordpress.org/download/) and unzip into C:/ProgramFiles/XAMPP/htdocs
7. Rename the WordPress folder to what you'd like your site name to be
8. Go into your WordPress files and find wp-config-sample.php. Rename to wp-config.php
9. Open wp-config.php in your code editor update the database name to whatever you named your database, change the user to root, and leave password as an empty string 
10. Go to your browser and type localhost/{your-site-name}
11. Choose your language, and on the next screen fill in your site title, username, password, and email. You should now be able to log in and see your WordPress dashboard

Sometimes I have errors logging in or opening all these pages, and that tends to be due to database errors.  When I set up my latest install of WordPress earlier this week, the password I set in my wp-config.php was not the same as the root password, which I set months ago and forgot about.

Also when I am typing this today I am not at home, I am at a cafe.  I cannot open localhost/phpmyadmin/ and I suspect it is because I am in a different place that is not whitelisted in my xampp set up. It is not super critical because it seems like I can still log into my site and work on it, but this could become problematic if I need to adjust something with my database.

### Instructions for logging into Wordpress (after it is set up)

I already forgot how to do this after a few days, so I am leaving this here for future me for when I inevitably forget again.

- open xampp
- start Apache and MySQL
- go to browser
- type localhost/{your-site-name}/wp-admin
- login

Now you are at your site's admin dashboard.
