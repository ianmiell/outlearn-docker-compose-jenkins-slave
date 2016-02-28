<!--
{
"name": "docker-compose-setup",
"version" : "0.1",
"title" : "Docker Compose Setup",
"description" : "Get setup with Docker Compose",
"homepage" : "https://github.com/ianmiell/outlearn-modules",
"freshnessDate" : 2015-07-08,
"license" : "CC BY 4.0"
}
-->
<!-- @section -->

# Docker Install

In this first part we're going to check you have the right version of Docker.

At your terminal, type 

```
docker version
```

and you should get some output that looks like this:

```
Client:
 Version:      1.9.1
 API version:  1.21
 Go version:   go1.4.2
 Git commit:   a34a1d5
 Built:        Fri Nov 20 13:16:54 UTC 2015
 OS/Arch:      linux/amd64

Server:
 Version:      1.9.1
 API version:  1.21
 Go version:   go1.4.2
 Git commit:   a34a1d5
 Built:        Fri Nov 20 13:16:54 UTC 2015
 OS/Arch:      linux/amd64
```

If you get output that looks like this:

```
docker: command not found
```

Then you need to install docker first.

If your version is older than 1.8, then you may come across problems later.

See [here](https://docs.docker.com/engine/installation/) for how to install the latest version of Docker.

<!-- @section -->

### Using Markdown

You may have already used Markdown. It's an awesome format for technical publishing. If you need a refresher, GitHub has a great overview.

<!-- @link, "url" : "https://help.github.com/articles/markdown-basics/", "text": "I know enough about Markdown." -->

GitHub-flavored markdown will work just fine, and you can preview your files in your favorite text editor or Markdown preview tool before uploading them to Outlearn for final proof-reading.

If you want to enrich your content with more features, this sample module has a few examples, including the Unfurled Link above, and the interactive features described in the next section.

> Block quotes get special styling on Outlearn. They turn into highlight boxes.


<!-- @section -->

# Images and videos

You can include images using Markdown syntax.

![sea](https://raw.githubusercontent.com/outlearn-content/outlearn-modules/master/assets/sea.jpg)


You can embed hosted videos in your paths using Outlearn @asset syntax. Here is a Vimeo video:

<!-- @asset, "contentType": "outlearn/video", "provider": "vimeo", "url": "https://player.vimeo.com/video/67325705" -->

And here is another one form YouTube:

<!-- @asset, "contentType": "outlearn/video", "provider": "youtube", "url": "https://www.youtube.com/embed/CmjeCchGRQo" -->

<!-- @section -->

# Let's make our module Fancy

If you want to get your audience to practice what you preach, give them a task.

```python
# Transpose a matrix:
>>> l = [[1, 2, 3], [4, 5, 6]]
>>> zip(*l)
[(1, 4), (2, 5), (3, 6)]

# Divide a list into groups of n:
>>> l = [3, 1, 4, 1, 5, 9, 2, 6, 5, 3, 5, 8]
>>> zip(*[iter(l)] * 3)
[(3, 1, 4), (1, 5, 9), (2, 6, 5), (3, 5, 8)]
```

<!-- @task, "text" : "Go and run these clever code examples on your own machine, lazy bones!"-->

And if they are making something worth sharing, why not let them share it with you?

<!-- @task, "hasDeliverable" : true, "text" : "Write and submit a haiku about your favorite compiler."-->

Then go ahead and test how much they know.

<!-- @multipleChoice -->

You can add a matrix and its transpose together only if

- [ ] The elements of the matrix are integers
- [ ] The matrix has an inverse
- [X] The matrix is a square matrix

Remember that two matrices can be added if they have the same number of rows and columns.

<!-- @end -->


If you need more details on how to author, please visit the [Outlearn Publishing Guide](https://www.outlearn.com/learn/outlearn/outlearn-publishing), where you'll find more examples along with the full specification for the format used by this sample module.
