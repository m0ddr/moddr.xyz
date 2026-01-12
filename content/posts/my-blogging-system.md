+++
title = "My Blogging System"
date = 2025-06-18
description = "A summary of the technologies used to run this blog"
taxonomies.tags = ["tech"]

[extra]
math = false
+++
I've recently been writing physical notes to track my thoughts. I actually did this after receiving some advice about organizing tasks: operate on the basis that if you have something important that needs to be done, write it down.

Since then, I've found that writing my thoughts and perspectives on things has helped me process ideas more clearly and deepen my knowledge of topics I'm exploring. There is just something therapeutic about putting thoughts into words, and I'm hoping that through regular practice I can become a more effective communicator. I'd like to establish myself as someone with useful insights in the areas I'm passionate about – whether that's Python, technology, games or the latest thing I'm obsessing over. So, this blog was born. 

## Starting again
Okay, admittedly, this isn't my first attempt at making a blog. Previously I had a blog to record my thoughts but it mostly sat empty. I was putting tremendous pressure on myself to write either totally unique content or professional-level articles. I think part of this was owing to an admittedly over-engineered theme I originally created called [lookr](https://github.com/m0ddr/lookr) *(now archived).*

I put a lot of time into creating something very professional and "modern-looking", and I personally think that set the precedent for writing high-level content.

I built this theme from a fork of [zola-hook](https://github.com/InputUsername/zola-hook) which had a very small footprint and a simple layout. The thinking behind this decision would be that I could escape the mental block of having to produce something approaching perfection.

## Static Site Generator
The static site generator I'm using currently is [Zola](https://www.getzola.org/). I was originally considering creating this site using a Python framework such as fastHTML but ultimately decided against it. Before switching to Zola, I was previously using [Hugo](https://gohugo.io/), however I have a few gripes with Hugo that gave Zola the edge. Primarily though, I like the fact that with Zola, you can overwrite anything set in the themes directory within your own directories. The themes directory and its contents just mimic the directory structure at the root of your project. With Hugo, a theme is sort-of required, because Hugo needs to know a layout/template that should be used to render your content. The vast majority of Hugo users prefer precrafted templates and layouts and whilst there are extensive themes to choose from, you lock yourself in after making a choice.

The other reason I prefer Zola is the template engine itself. Zola uses a template engine called [Tera](https://keats.github.io/tera/) which was designed to have a lot in common with Django templates I'm used to working with. So it all just felt more natural and familiar.

## Hosting
For this blog iteration, I've decided to use GitHub Pages. That way I don't have to administer a Linux VPS in my free time, handling expiring certificates etc. Originally when I had a few different services it made sense to deploy the generated files to the vps using SSH and rsync, but I've since been trying to simplify my workflows. The simplicity of git push to live site as well as the benefits of having automatic SSL and global CDN out of the box outweigh the benefits of custom server configuration (*at least for now*). 

## Post Tags
In previous iterations of my blogs, I didn't actually make an effort to categorise or sort my posts. This time I'm making use of tags. Conceptually, I'm sort of using tags like categories, but since Zola handles taxonomies all the same, I think it is more modern. All tags can be viewed by visiting the [/tags](/tags) page.

## Images
I recently watched an [interesting video](https://www.youtube.com/watch?v=N16q_JVIPqQ&t) about optimizing images. Essentially I'm using the same strategy, utilizing object storage via [Cloudflare R2](https://developers.cloudflare.com/r2/).

I didn't opt to use [Bespoke](https://github.com/tmcw/bespoke) (the tool referenced in the video). ImageMagick is more than capable of converting images to the desired format and is available on most Linux systems. My workflow involves converting any images I use in posts, with a bash script to handle the conversion and upload process. I've created a custom shortcode for this blog that serves images from object storage using WebP with JPEG as a fallback.

**Example:**

{{webp_jpg(src="test.jpg", alt="an image of a circuit board to serve as an example of image delivery via a custom content delivery network", caption="Example low-res image being served via my custom image CDN")}}

## Bit Rot Mitigation
Lastly, to combat bit rot of my posts, I use a custom-built dead link checker called link-sweep. I designed it to be runnable via GitHub actions. 
{{gh_repo(owner="m0ddr", repo="link-sweep")}}
The tool itself is non-destructive by default and scans any Markdown files in a target directory, reporting any links that time out or return a response code over 400. *4xx codes are client errors and 5xx are server errors*. It's likely that I'll expand upon this tool, perhaps to also capture my image optimization workflow or make any further optimizations as they reveal themselves with extended usage. For now though, I'm generally happy enough with the results.
