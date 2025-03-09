---
title: "Migrating to Hugo from Gatsby"
slug: "migrating-to-hugo-from-gatsby"
date: "2024-07-10T21:33:19+01:00"
draft: false
---

Hey folks! For the past number of years I've had a gatsby powered site sitting on omalley.dev. I bought the domain when `.dev` first got released and have always wished to do something more with it. I started writing over COVID but never published much outside of an article on npm-overrides. While I would take the time to start articles I would inevitably get side tracked fixing gatsby realted issues. My hope with this move to Hugo I will get to spend more time writing and less time triging.

Here is a summary of the steps I followed to migrate a blog from Gatsby to Hugo:

## Initial setup
My sites code lives in github and I host on netlify. My domains DNS is handed off to netlify's DNS servers and I'm pretty happy with how it's worked the past few years so I don't feel like changing this.

My first step is to install Hugo. I split my dev between Mac OS and Windows. On Mac I used `brew` to install Hugo, On windows I used `winget`

Mac: `brew install hugo`

Windows: `winget install Hugo.Hugo.Extended`

Once installed I used the following to scafold out my site.

`hugo new site new.omalley.dev --format toml`

I initially used JSON as my config lanugage of choice but swapped to TOML as it allowed for easier multi-line strings and less typing to make changes.

## Theme setup
I have to admit after spending hours trying to theme gatsby the hugo submodule approach to themes was a breath of fresh air. I've chosed the very popular Papermod theme as it has most of the features I want and looks close enough to my original theme that it shouldn't be so jaring for anyone coming to the site.

In the site directory run:

`git submodule add --depth=1 https://github.com/adityatelange/hugo-PaperMod.git themes/PaperMod
git submodule update --init --recursive`

and then update your hugo.toml

`theme = "PaperMod"`

## Netlify Integration
I really love Netlify for it's ease of use. It's been my go to providor for spinning up new sites or small projects. I only had to add a few things to a `netlify.toml` file to get the branch deploys working as expected. Other than that it worked OOTB

```
[context.deploy-preview]
command = "hugo --gc --minify --baseURL ${DEPLOY_PRIME_URL}"
```

## Content Migration
Thankfully this was reletively simple. Both Hugo and Gatsby use markdown for their posts and the headers are similar enough that I could just copy paste the content. I did have to play with the URL generator to map the original URLs so that they'll still work. Additioanlly I had to make a breaking change to where the old images are stored and linked. Rather than being in the static/images folder they now live alongside their posts.

## Observability
In order to get some additional info as to who may or may not be viewing my site I looked for some "free" analytics. I could have continued to use Google Analytics, but I wanted to see what was available that could be more privacy focused. I settled on [PostHog](https://posthog.com/) as I've heard good things and they were relatively easy to integrate by dropping in a script tag.

I created a new folder in my hugo site called `layouts/partials` and added a new file called `extend_head.html`. This allows me to drop in the PostHog script to the site without having to manually edit the theme.

## Wrapping Up
With that the migration is complete. I'm happy with the results and I hope you are too. If you have any questions or comments please feel free to reach out.
