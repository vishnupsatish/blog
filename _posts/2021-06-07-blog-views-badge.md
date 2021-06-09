---
title: Blog Post Views on a Static Blog For Free
date: 2021-06-07 20:50:00 -0400
category: [programming, tools]
tag: [tools, meta, development]
---

## Background

I wanted to create a blog, and do so for free without being bound by a service that holds my content. (GitHub doesn't count, since the blog files and the posts are saved locally, and I can migrate easily to another git provider, or even a self-hosted git service if I wanted to).

I set up the blog using the [Chirpy](https://github.com/cotes2020/jekyll-theme-chirpy){:target="_blank"} Jekyll theme and I wanted a method to show blog post views. The method that was suggested on various articles and forums was to use Google Analytics' pageviews feature. I didn't want to do this because (1) Google Analytics has privacy concerns and (2) Firefox's Enhanced Tracking Protection feature (enabled by default) does not allow requests to Google Analytics to be made.

You might have noticed the "visitors" badge on every one of my posts. It looks something like this:

![](https://res.cloudinary.com/dnwczwamg/image/upload/vishnu-blog/posts/2021-06-07-blog-views-badge/badge_iay6su.svg)

This is my method of showing page views, and here's how it's done (for free).

## Visitors Badge

There is a handy web application that allows you to embed a visitors badge in Markdown or HTML for free, and statically. The application itself is made using Flask.

[GitHub Repo](https://github.com/jwenjian/visitor-badge){:target="_blank"}

I suggest that you don't use the public instance available. Instead, host the code on your Glitch instance.

## Hosting the Application

Glitch is a web application hosting platform, similar to Heroku, but it is easier to set up applications on Glitch.

[Glitch](https://glitch.me){:target="_blank"}

First, create an account on Glitch (or login) then navigate 

Then, navigate to your dashboard, and in the top-right corner, click "New Project" and "Import from GitHub" as is shown below.

![Step 1](https://res.cloudinary.com/dnwczwamg/image/upload/vishnu-blog/posts/2021-06-07-blog-views-badge/Screen_Shot_2021-06-08_at_7.12.34_PM_kxq7y2.png){:width="50%"}

Paste the link "https://github.com/jwenjian/visitor-badge" into the dialogue the appears and wait for the project to load. Then, navigate to the project's page by pressing the "Show" button in the top-left, then pressing "In a new window".

![Step 2](https://res.cloudinary.com/dnwczwamg/image/upload/vishnu-blog/posts/2021-06-07-blog-views-badge/Screen_Shot_2021-06-08_at_7.17.53_PM_uvbqmw.png){:width="50%"}

## Adding the Badge

Then, you'll a page that looks like this:

![Step 3](https://res.cloudinary.com/dnwczwamg/image/upload/vishnu-blog/posts/2021-06-07-blog-views-badge/Screen_Shot_2021-06-08_at_7.21.19_PM_eqspee.png){:width="50%"}

Click on "Docs" in the top-right the follow the instructions to embed your badge using Markdown or HTML.

## Recommended: Keeping Application Awake

There is a catch though; if you're using Glitch's free tier (as I am), the application will go to sleep after five minutes of inactivity. You can use a cron job (a repeated task) to stop the application from going to sleep.

To read more about cron, you can read its [Wikipedia article](https://en.wikipedia.org/wiki/Cron){:target="_blank"}.

We'll be using cron to run a script every four minutes. That script will be a Python script that uses Selenium (a browser automation tool) to visit the Glitch application, therefore ensuring that it never goes to sleep, as long as your computer is awake.

You will need the following technical knowledge.
- Using the CLI
- Python
- A basic understanding of Selenium

First, you need to download the Firefox browser and "geckodriver", a browser automation driver that Selenium uses to interact with Firefox. 

You can install "geckodriver" from the following link:

[Install geckodriver](https://github.com/mozilla/geckodriver/releases){:target="_blank"}

Remember the path where "geckodriver" is installed.

Then, create a Python file with the following content:

```python
#!/usr/local/bin/python3
from selenium import webdriver 
from selenium.webdriver.firefox.options import Options
firefox_options = Options()
firefox_options.add_argument('--headless')
driver = webdriver.Firefox(executable_path='/path/to/geckodriver', options=firefox_options, service_log_path='/dev/null' if on Linux/Mac or 'nul' if on windows)
driver.get('your Glitch application URL')
driver.quit()
```

Then, give the Python script the necessary permissions to be run as executable, like the following.

```shell
chmod +x path/to/script.py
```

Use your cron tool to create a task that runs every four minutes. It can look like this on Mac (if you're using crontab, the in-built cron tool on OS X).

```shell
MAILTO=""

*/4 * * * * cd path/to/directory/of/python/script && ./python_file_name.py 2>&1
```

If you're using a Mac, there may be cron permissions issues, and if the task does not seem to work, you may need to drag the `cron` executable from `/usr/sbin` into "Full Disk Access" in the Security and Privacy section in System Preferences.

If you want to ensure that your cron job runs even when your computer is asleep or closed, you can run your cron on an online service (none of the free services seemed to work for me though, since I couldn't run custom scripts).

And that's it!
