{:title "Embedding Gorilla-repl worksheet in a blog post"
 :layout :post
 :draft? false
 }


[Gorilla-repl](http://gorilla-repl.org) is a toolset from the Clojure stable, which enables us to interact, visualize and share Clojure code as a worksheet. For those familiar with IPython notebooks (now called Jupyter), Gorilla-repl is the Clojure equivalent of the same. I find that it is an excellent environment for sharing and discussing data science.

<iframe src="https://player.vimeo.com/video/87118206" width="640" height="400" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen></iframe>
<p><a href="https://vimeo.com/87118206">Gorilla REPL introduction</a> from <a href="https://vimeo.com/user25262253">JonyEpsilon</a> on <a href="https://vimeo.com">Vimeo</a>.</p>


I wanted the ability to add a new blog post, where the contents of the post are an (externally hosted) Gorilla repl worksheet, such as on Github. I discovered the [Cryogen blog engine](http://cryogenweb.org/) (again, written in Clojure), and I was able to add support to host Gorilla-repl worksheets to the same.

Currently this work remains as a pull request on the Cryogen project.

Documentation on how to create a gorilla-repl supported Cryogen blog can be [found in the docs](https://github.com/shark8me/cryogen/blob/gorillarepl-worksheets/doc/gorilla-repl.md). An example of a hosted worksheet can be [found here](http://kirank.in/posts-output/2016-01-06-notebook/).


