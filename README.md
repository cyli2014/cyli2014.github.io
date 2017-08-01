# cyli2014.github.io

[A personal home page][homepage] forked from a Jekyll theme [Centrarium][centrarium] by Ben Centra. 

It works on GitHub Pages. If you are interested in this theme you can go to [Centrarium][centrarium-github] or fork my repository directly and turn it into your own home page.

## Configuration

All configuration options can be found in `_config.yml`. I would like to introduce some customization methods below.

### Site Settings

* __title:__ The title for your site. Displayed in the navigation menu, the `index.html` header, and the footer. You need to change it into your own title. 
* __subtitle:__ The subtitle of your site. Displayed in the `index.html` header and below your title in actual page. Change it if needed.
* __email:__ Your email address, displayed with the Contact info in the footer. 
* __name:__ Your name. _Currently unused._
* __description:__ The description of your site. Used for search engine results and displayed in the footer. I use it as copyright messages for fun.
* __baseurl:__ The subpath of your site (e.g. /blog/). __Set it to blank if you use GiuHub domain directly and change it only when you totally understand the structure of your site!__ If the css design disappears with only words left you may probably need to check this setting. 
* __url:__ The base hostname and protocol for your site. If you use GitHub domain directly change it into _http://yourusername.github.io_.
* __cover:__ The relative path to your site's cover image.
* __logo:__ The relative path to your site's logo. Used in the navigation menu instead of the title if provided.

### Social Settings

Your personal social network settings are combined with the social sharing options. In the __social__ section of `_config.yml`, include an entry for each network you want to include. For example:

```yml
social:
  - name: Twitter                         	# Name of the service
    icon: twitter                         	# Font Awesome icon to use (minus fa- prefix)
    username: Darwin632063265                	# (User) Name to display in the footer link
    url: https://twitter.com/Darwin632063265	# URL of your profile (leave blank to not display in footer)
    desc: Follow me on Twitter            	# Description to display as link title, etc
    share: true                           	# Include in the "Share" section of posts
```

### Category Descriptions

You can enhance the `posts.html` archive page with descriptions of your post categories. See the __descriptions__ section of `_config.yml`. Below is an adding example.

```yml
# Category descriptions (for archive pages)
descriptions:
  - cat: diary
    desc: "Life experience and thoughts."

  - cat: project
    desc: "Posts describing projects and instructions."

  - cat: welcome
    desc: "Welcome pages."

  - cat: example
    desc: "An example category."
```

### Navigation

You can change the `About`, `Post` and `Projects` navigation columns by modifying corresponding `about.md`, `posts.html` and `projects.md`. The configuration lines are at the beginning of each page:

```yml
---
layout: page
title: Projects
permalink: /projects/
---
```

Words on navigation bar will change after you edit the `title` above. If you want to add navigation columns, just add new files in the root of your repository. Both `.md` and `.html` work. If you don't need it anymore, delete the file directly.

### Adding posts

You need to edit your post by `markdown` and obey a file name regulation like `20xx-xx-xx-title.markdown`. Then add it to `_posts` folder directly. You can configure your post by editing the beginning lines like:

```yml
---
layout: post
title:  "Welcome to Darwin's Home!"
date:   2017-07-28 10:57:59
author: Darwin Li
categories: Welcome
tags:	welcome navigation
cover:  "/assets/welcome.jpg"
---
```

You can choose a category defined above. Tags are not self-generated and need editing by yourself. You can also copy your own cover to `asset` folder and modify the relative path.

## Acknowledgement

Special thanks to [Centrarium][centrarium] by Ben Centra. Detailed introduction of this theme can be found on his [repository][centrarium-github].

[homepage]:		https://cyli2014.github.io
[centrarium]:	http://bencentra.com/centrarium/
[centrarium-github]:	https://github.com/bencentra/centrarium