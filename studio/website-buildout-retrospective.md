---
title: "Website Buildout Retrospective"
post_status: publish
post_excerpt: "As we launch our public website, we want to share a bit about how it was built, and how we're managing it while building a game."
menu_order: 0
taxonomy:
    category: "studio"
    post_tag:
        - studio
        - tech
        - wordpress
        - retrospective
    hashtags:
        - github
        - wordpress
        - appalachiainteractive

---

As we launch our public website, we want to share a bit about how it was built, and how we're managing it while building a game.  The point isn't to document the build, or to publish general tech tutorials.  More, it's meant to show how we're solving the problem of running and maintaining a public website, when *really* you're trying to build a video game.

Hint: we're doing it live in production.

And to be abundantly clear about this this series of posts (and this blog), there won't be any affiliate links, no advertisements — no attempt or possibility for us to make money or sell your data here.  That's just never something we're going to do.  This is really just to help someone who finds themselves in the same spot we were.

## The challenge

I've spent nearly all of my professional career leading server-side teams on the back-end.  UI, UX, anything in the same *room* with JavaScript — I really very interested in it, and didn't have a knack for it.  I also wasn't impressed by the technology.  It (mainly ASP.NET, jQuery, page builders,e tc.) just didn't seem to be able to keep people from having to spend hours doing trivial things like minor CSS adjustments.  Any time I had to be involved in something that would have a UI component, I would define an API, give it to the front-end developers, and run away
screaming before they could trap me in their world of Javascript-fueled insanity.

With a small studio, we really can't afford to lose time on things like that.  And as a perfectionist, it's dangerous because I will totally get sucked in if no one is there to pull me out.  So when we started building this website, being able to scale the management of it was an important consideration.

## The solution

While the build is still in progress, there *have* been a few big wins.  These are tools or even ideas that feel like they make us more productive than we have any business being.  Some of these will get a post in the future — but here's a broad overview:

### GitHub

We're always going to solve problems like programmers, so obviously most of our website content is actually pulled directly from GitHub.  In fact, if I'm not mistaken, you can see this post (*yes, ***this*** one*), right [here](https://github.com/AppalachiaInteractive/com.appalachia.blog/blob/main/studio/website-buildout-retrospective.md).

Now how cool is *that*?  Basically, we use WordPress plugins to pull the content into WordPress so that it looks nice and fancy.  But I'm writing this post in Markdown, using VS Code — like a good little coder.  Staying in IDE is a big win for us so we wanted to be able to work like this.

We also really like the forced accountability and transparency.  You will always be able to see what we write, and all of the iterations.  We're never going to be able to change our position on something, or hide something we said, without it being visible.  And if we do, you can dig through the history and expose our fraud.  But we won't ;)

That means something to us though.  For example, if someone takes issue with something we add or remove from our *Causes* section, they can go back and review everything we've ever written about what we care about.  If we're frauds, you have all the evidence you need to find it.  And that forced accountability is hopefully a demonstration that we are trying to be straightforward and honest from the jump.

### SVG over PNG

We make almost all of our web art in SVG, using Adobe Illustrator.  We tried using Inkscape for a long time, but it really is just not stable.  I hate to say that about a really cool open-source project, but if you're making complex SVGs and need to be able to export multiple assets from a single file, etc - you have to use Illustrator.  And if you want to be able to handle the dynamic sizing of the web, you need SVG instead of PNG.  Look at our [home page](https://appalachiainteractive.com) - its *all* SVG.  That's the only way you're going to get small file sizes with great quality on huge images.  You do need to pay attention to SVG optimization and how to author a good SVG file in the first place, but that's a problem that can be solved.  You are not going to have a great time solving the problem of making dozens of 3000px wide image files very small and fast-loading.

We loved reading Raymond Schwartz's [Understanding and Manually Improving SVG Optimization](https://css-tricks.com/understanding-and-manually-improving-svg-optimization/).  It's dense but it will show you the *what* to do.

[SVGO](https://github.com/svg/svgo) is also a killer tool that basically does everything in the post above, and more.  We use it on the command line as part of our custom dev environment, but here is a web UI available [here](https://jakearchibald.github.io/svgomg/) if you want to try it out.


### WordPress

WordPress seems to have been a wonderful choice of platform.  We're using WordPress.com hosting (not self-hosting our own installation), and that was a lucky choice.  The support is really great, very professional and quick.  They *definitely* add some abstraction in between you and the web server, but if you know what a web server does and how to work with one, you can kind of work through the abstraction effectively enough.

There's a few plugins we used that really helped us.

#### Advanced Custom Fields

```
Customize WordPress with powerful, professional and intuitive fields.
```

[Advanced Custom Fields](https://wordpress.org/plugins/advanced-custom-fields/) instantly helped us understand WordPress.  In a moment, we got it - *posts have fields*.  We realized that WordPress was essentially a relational data store for the pages we made, and this plugin let us add new columns to the Post page in a sense.  I say 'in a sense', because really WordPress uses a key-value table named `wp_postmeta` to store additional custom fields for your posts.  But as soon as we understood this, adding custom fields to enable the automation we needed was easy.

#### Code Snippets

```
An easy, clean and simple way to run code snippets on your site. No need to edit to your theme's functions.php file again!
```

[Code Snippets](https://wordpress.org/plugins/code-snippets/) was nice, because we didn't have true web server access, and we didn't actually want to learn anything about how WordPress works.  We did, in the end, but this plugin made it easy to basically copy and paste PHP snippets, and then have them work on our site, without needing to really dig into the depths of WordPress.

#### External Markdown

```
Include and parse markdown files from external web sources like GitHub, GitLab, etc.
```

[External Markdown](https://wordpress.org/plugins/external-markdown/) is awesome.  It allows you to embed external Markdown right into your WordPress page.  Most of our pages, like our [About](https://appalachiainteractive.com/about) page, have almost no WordPress content other than this plugin.  It points right to our GitHub, where it pulls Markdown content and caches it so that we can write the content of our website using VS Code, and not have to actually log into WordPress to modify it.  This is a great benefit to our workflow.

#### Git it Write

```
Publish markdown files present in a Github repository as posts to WordPress automatically
```

If you liked the idea of External Markdown, you're going to love [Git it Write](https://wordpress.org/plugins/git-it-write/) is awesome.  This plugin actually posts to WordPress for you, by setting up a webhook in your GitHub repo.  When I commit this Markdown file that I'm currently writing, Git it Write will scan the YAML front-matter and parse it into post metadata, and then actually either create or update my post.  So yes, I can post to WordPress, without logging into WordPress.  That's a nice time savings.

#### SVG Support

```
Upload SVG files to the Media Library and render SVG files inline for direct styling/animation of an SVG's internal elements using CSS/JS.
```

[SVG Support](https://wordpress.org/plugins/svg-support/) enables MP3 support for WordPress.  Wait, no.  It enables SVG support for WordPress.  There are obviously some security issues with SVG, as it is XML, but if you're authoring the content you don't really need to worry, as long as you have full control over your source files.

#### Media Library Folders for WordPress

```
Gives you the ability to adds folders and move files in the WordPress Media Library.
```

[Media Library Folders](https://wordpress.org/plugins/media-library-plus/) definitely helped us.  I really don't like something telling me what folder structure I have to use, and WordPress media library uses an absurd setup.  I have no idea why anyone wants to group media by the year and month you uploaded it.  Nuts.  But plugin lets you upload files from your own folder structure on the server.  So if you can FTP files right to your WordPress server, it will upload them into the Media Library.  You can maintain them in a structure you want both locally and on the server, which is nice.  You do need to get FTP deployment working to take advantage of this, though.

#### Yoast SEO

```
The first true all-in-one SEO solution for WordPress, including on-page content analysis, XML sitemaps and much more.
```

I dislike SEO (search engine optimization) and most of the companies/consultancies who specialize in it.  The worst of them can be vampires, pushing SEO on every mom-and-pop company, despite that the consultants themselves don't really understand search algorithms, and have absolutely no way to confidently predict that the mom-and-pop and is going to make back the money they invest.  They're the technology equivalent of buying Holy Water from a late-night infomercial.  But [Yoast](https://yoast.com/) seems different to me. They don't talk about dark and mystic SEO black-magic. Their features are described well, they say when something is a toss-up on how effective it is, and they don't vaguely threaten you that doing or not doing a thing is going to 'harm your website'.  They also have a well-engineered WordPress plugin so I was able to automate all of the updates for each post.  Basically, when I publish a blog post, it will already have the OpenGraph, Twitter, etc. metadata and image set.  That's a big time savings for me.

### Elementor

```
The Elementor Website Builder has it all: drag and drop page builder, pixel perfect design, mobile responsive editing, and more. Get started now!
```

[Elementor](https://elementor.com/) is a WordPress plugin, but its also basically a full UI replacement for page building, so it gets its own section.  It was the big-daddy of all plugins for us.  It's the best page-builder I've ever seen.  It saved us so much time while also really unlocking our creativity.  The desktop/tablet/mobile breakpoint & preview system works wonderfully.  Can't say enough really.  I would not build another site without it.

It has its own add-on ecosystem.  The most important add-on we used is below.

#### Dynamic Conditions

```
Activates conditions for dynamic tags to show/hides a widget.
```

[Dynamic Conditions](https://wordpress.org/plugins/dynamicconditions/), an elementor add-on, adds a simple ability to toggle on or off whether a component should appear.  We use this extensively to control when and where things like menus show up.  Really easy to create complex conditions and there is a little icon that helps you find conditional elements.

## What else?

Hold your horses there, Tex.  We'll be publishing more details about some of these items, and some we didn't mention.  Keep an eye out, but don't hold your breath - we're not a tech blog!
