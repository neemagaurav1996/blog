---
layout: post
title:  How to migrate a WordPress website without any plugin?
date:   2021-01-27 17:58:17 +0530
categories: [Blogging, Article]
permalink: migrate-wordress-website-without-plugin
tags: wordress migration docker linux website
image: images/article-images/wordpress-migrate.png
image_alt: migrate-wordress-website-without-plugin
comments: true
canonical_url: https://gaurav-neema.medium.com/how-to-migrate-a-wordpress-website-without-any-plugin-6c258d33121d
clean_date: Jan 27, 2021
duration: 2 min read
description: Migrating a WordPress website sounds so easy, and really is. But there are few caveats that you need to keep in mind if you are doing it for the first time. This story will guide you on how to migrate the WordPress Website easily without the use of any plugin.
display_image: images/wordpress-blue.svg
---

## {{ page.title }}
<div class="article-info muted-text">
    <span class="published-on">Published on <a rel="noopener" href="{{ page.canonical_url }}" target="_blank">Medium</a> on {{ page.clean_date }}</span>
    <span class="duration"><i class="icon-clock"></i> {{ page.duration }}</span>
</div>

Using Docker environment

<!--more-->

<figure>
	<img class="article-image" src="{{ page.image }}" alt="{{ page.image_alt }}" width="200">
	<figcaption class="article-image-caption">Photo by <a href="https://unsplash.com/@tambuzi?utm_source=unsplash&amp;utm_medium=referral&amp;utm_content=creditCopyText" class="bv iv" rel="noopener nofollow">Jordi Fernandez</a> on <a href="https://unsplash.com/s/photos/migration?utm_source=unsplash&amp;utm_medium=referral&amp;utm_content=creditCopyText" class="bv iv" rel="noopener nofollow">Unsplash</a></figcaption>
</figure>


### Requirements

- docker
- docker-compose

### Steps

**1. Installing WordPress**

Setup WordPress using docker. Here is the and `Dockerfile` and `docker-compose` file for the same. Please change the username, password, database names accordingly:

<script src="https://gist.github.com/neemagaurav1996/3b1832eb3ecbea0c1248eab20f1e47c2.js"></script>
<div class='gist-caption'>Dockerfile- You can change the PHP variables here</div>

<script src="https://gist.github.com/neemagaurav1996/866c8df3a6d6d62e2fa7ed084d99eeb1.js"></script>
<div class='gist-caption'>docker-compose.yml</div>

Build and start up the application using the command:
`docker-compose up -d`

**2. Database changes**

Backup the database from the old environment and restore it to the new environment inside docker. The links and certain URLs will still point to the old environment in certain tables in the database. Use the following queries to change the URLs:

<script src="https://gist.github.com/neemagaurav1996/b7fdf8be5377ca7831f45cbdd57843f8.js"></script>
<div class='gist-caption'>Replace the old and the new URL values</div>

**3. Replace the wp-content folder**

`wp-content` is the directory where all the changes you made to the website goes. This includes imports, pages, PHP code, etc. Copy the folder from the old environment and paste it into the new environment after deleting the existing one.

**4. Finalizing stuff**

I was using Elementor page builder which provides the option to regenerate CSS, sync files and replace the URLs. If you are not using Elementor, you can skip this step or perform the equivalent step if you are using another page builder.
<figure>
	<img class="article-image-2" src="images/article-images/wordpress-migrate-1.png" alt="Regenerate Images">
</figure>

<figure>
	<img class="article-image-2" src="images/article-images/wordpress-migrate-2.png" alt="Replace URLs">
</figure>


**5. Fire Up!**

Clear up the browser cache and fire up the website!

That’s all! These are some basic steps that allow you to easily migrate the wordpress website to a new domain.