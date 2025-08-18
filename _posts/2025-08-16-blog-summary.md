---
layout: post
title: Blogs syntax summary
date: 2025-08-16 18:30:16
description: start to write blog posts
tags: latent dynamics, neural population
categories: sample-posts
thumbnail: assets/img/9.jpg
featured: true
disqus_comments: true
giscus_comments: true
related_posts: false
related_publications: true
citation: true
toc:
  beginning: true
  sidebar: left
---


<hr>
Primary text enters here. For a new paragraph, no need for a specified new-line character.

Here's how to embed a weblink: [Pinterest](https://www.pinterest.com), or Direct trade <a href="https://en.wikipedia.org/wiki/Cold-pressed_juice">cold-pressed</a>. Both methods have the same effect.

#### Hipster list

- brunch
- fixie
- raybans
- messenger bag

#### Check List

- [x] Brush Teeth
- [ ] Put on socks
  - [x] Put on left sock
  - [ ] Put on right sock
- [x] Go to school

For indented section:
> We do not grow absolutely, chronologically. We grow sometimes in one dimension, and not in another, unevenly. We grow partially. We are relative. We are mature in one realm, childish in another. 
> —Anais Nin

<hr>

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/9.jpg" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/7.jpg" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    A simple, elegant caption looks good between image rows, after each row, or doesn't have to be there at all.
</div>

Images can be made zoomable.
Simply add `data-zoomable` to `<img>` tags that you want to make zoomable.

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/8.jpg" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/10.jpg" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>

The rest of the images in this post are all zoomable, arranged into different mini-galleries.

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/11.jpg" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/12.jpg" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/7.jpg" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>

<hr>
````markdown
```c++
code code code
```
````

```python
 j = 1
 for i in range(8):
    print(j + i)
```

1. We can put fenced code blocks inside nested bullets, too.

   1. Like this:

      ```c
      printf("Hello, World!");
      ```

   2. The key is to indent your fenced block in the same line as the first character of the line.


The keyword `linenos` triggers display of line numbers.
Produces something like this:

{% highlight python linenos %}

 j = 1
 for i in range(8):
    print(j + i)

{% endhighlight %}

<br>
For displaying math, for example:

$$
\sum_{k=1}^\infty |\langle x, e_k \rangle|^2 \leq \|x\|^2
$$

or 

\begin{equation}
\label{eq:cauchy-schwarz}
\left( \sum_{k=1}^n a_k b_k \right)^2 \leq \left( \sum_{k=1}^n a_k^2 \right) \left( \sum_{k=1}^n b_k^2 \right)
\end{equation}

and by adding `\label{...}` inside the equation environment, we can now refer to the equation using `\eqref`.

<br>
Could also include Twitter:

# Tweet

An example of displaying a tweet:
{% twitter https://twitter.com/rubygems/status/518821243320287232 %}

# Timeline

An example of pulling from a timeline:
{% twitter https://twitter.com/jekyllrb maxwidth=500 limit=3 %}

# Additional Details

For more details on using the plugin visit: [jekyll-twitter-plugin](https://github.com/rob-murray/jekyll-twitter-plugin)


<br>
For post-citation:

This is an example post that can be cited. The content of the post ends here, while the citation information is automatically provided below. The only thing needed is for you to set the `citation` key in the front matter to `true`.

<br>
For post-bibliography:

That means simple citation like {% cite einstein1950meaning %}, multiple citations like {% cite einstein1950meaning einstein1905movement %}, long references like {% reference einstein1905movement %} or also quotes:

{% quote einstein1905electrodynamics %}
Lorem ipsum dolor sit amet, consectetur adipisicing elit,
sed do eiusmod tempor.

Lorem ipsum dolor sit amet, consectetur adipisicing.
{% endquote %}

If you would like something more academic, check the [distill style post]({% post_url 2018-12-22-distill %}).


<br>
Table of contents:
To add a table of contents to a post, simply add

```yml
toc:
  beginning: true
```

to the front matter of the post. The table of contents will be automatically generated from the headings in the post.

To add a table of contents to a post as a sidebar, simply add

```yml
toc:
  sidebar: left
```

to the front matter of the post. The table of contents will be automatically generated from the headings in the post. If you wish to display the sidebar to the right, simply change `left` to `right`.

If you want to learn more about how to customize the table of contents of your sidebar, you can check the [bootstrap-toc](https://afeld.github.io/bootstrap-toc/) documentation. Notice that you can even customize the text of the heading that will be displayed on the sidebar.


<br>
Videos:

This is an example post with videos. It supports local video files.

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include video.liquid path="assets/video/pexels-engin-akyurt-6069112-960x540-30fps.mp4" class="img-fluid rounded z-depth-1" controls=true autoplay=true %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include video.liquid path="assets/video/pexels-engin-akyurt-6069112-960x540-30fps.mp4" class="img-fluid rounded z-depth-1" controls=true %}
    </div>
</div>
<div class="caption">
    A simple, elegant caption looks good between video rows, after each row, or doesn't have to be there at all.
</div>

It does also support embedding videos from different sources. Here are some examples:

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include video.liquid path="https://www.youtube.com/embed/jNQXAC9IVRw" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include video.liquid path="https://player.vimeo.com/video/524933864?h=1ac4fd9fb4&title=0&byline=0&portrait=0" class="img-fluid rounded z-depth-1" %}
    </div>
</div>






