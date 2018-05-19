# Jekyll

Today I learned how to make a `"post"` using jekyll as a static web generator.


## Create a new post

To create a new post, all you need to do is create a file in the `_posts` directory. 
How you name files in this folder is important. 
Jekyll requires blog post files to be named according to the following format:

```
YEAR-MONTH-DAY-title.MARKUP
```

Where `YEAR` is a four-digit number, `MONTH` and `DAY` are both two-digit numbers, and MARKUP 
is the file extension representing the format used in the file. 
For example, the following are examples of valid post filenames:

```
2011-12-31-new-years-eve-is-awesome.md
2012-09-12-how-to-write-a-blog.md
```

## Include Images/Assets

That might be a problem. I'm facing problems linking github content from one page to other and then 
when I open the site through the *custom domain* the link simply do not work.

> There are a number of ways to include digital assets in Jekyll. 
> One common solution is to create a folder in the root of the project directory called something like assets, 
> into which any images, files or other resources are placed. 
> Then, from within any post, they can be linked to using the site’s root as the path for the asset to include. 
> Again, this will depend on the way your site’s (sub)domain and path are configured, 
> but here are some examples in Markdown of how you could do this using the *absolute_url* filter in a post.

On Jekyll [documentation](https://jekyllrb.com/docs/posts/) it says:

```
![My helpful screenshot]({{ "/assets/screenshot.jpg" | absolute_url }})
```

So, my question is, will *absolute_url* works on either github repository and github pages 
(using jekyll for generating the static web site) ?

I do not know answer this question. I'm gonna test then, I will update this **TIL**
