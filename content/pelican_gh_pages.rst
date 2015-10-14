Using Pelican with GitHub pages
===============================

:date: 2015-10-04 13:21

The very first problem of publishing this website was that I wanted to use `Pelican <http://blog.getpelican.com>`_ for content generation and I wanted to use `GitHub pages <https://pages.github.com>`_ for serving. GitHub pages serves your content from ``index.html`` file located in your project's root directory. Pelican, by default, uses ``output/`` folder to put generated content in, where your website's ``index.html`` is located.

Let's check how current Pelican project folder tree looks like (here are not shown files, which are not essential for us now, e.g. ``pelicanconf.py``).

.. code-block:: shell

    $ tree trimailov.github.io

    trimailov.github.io # root directory
    ├── Makefile
    ├── content # our source content
    │   └── first_post.rst
    ├── output # generated html files from pelican
    │   ├── CNAME
    │   └── index.html
    └── theme
        ├── static
        │   └── css
        │       └── main.css
        └── templates
            ├── base.html
            └── index.html

You can see that ``CNAME`` and ``index.html`` are both one layer beneath than GitHub pages expect.

git submodule way
-----------------

To solve this I decided to solve it by using ``git-submoule``. Basically, we'll create extra ``git`` repository inside ``output/`` folder and name it's remote repository in GitHub ``trimailov.github.io``.

You can do this like that:

.. code-block:: shell

   # create folder for pelican project
   $ mkdir trimailov-source
   $ cd trimailov-source
   $ git init

   # create folder for output (or let pelican generate it)
   $ mkdir output
   # create git repository inside output/ folder
   $ git init

   # go back to trimailov-source folder
   $ cd ..
   # add submodule to it
   $ git submodule add output

   # .gitmodules file is created
   # set url to output/ folder's github repository
   $ cat .gitmodules
   [submodule "output"]
       path = output
       url = git://github.com/trimailov/trimailov.github.io.git

After this you'll have 2 github repositories. One for your source and another for your output, e.g. `source <https://github.com/trimailov/trimailov-source>`_ and `output <https://github.com/trimailov/trimailov.github.io>`_ (output is the GitHub pages repository).

HTML meta redirect way
----------------------

There will be no 'output' repositories. Your 'source' will be your main repository (named e.g. ``trimailov.github.io``) where 'fake' ``index.html`` file will be placed, which will redirect to your output directory's ``index.html``.

I have not tried this method, and it may produce some url linking problems, and possibly your website will have ``/output`` appended to your domain.

.. code-block:: html

    <html>
        <head>
            <meta http-equiv="refresh" content="0; url=https://github.com/trimailov/trimailov.github.io/output/index.html">
        </head>
    </html>


