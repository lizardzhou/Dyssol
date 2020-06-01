# Dyssol Help Documentation
Help documentation for a dynamic simulation software for solids processes.

Install and configure
---------------------

In order to edit this documentation with [Sphinx](https://www.sphinx-doc.org/en/master/index.html) , you should [install](https://www.sphinx-doc.org/en/master/usage/installation.html) it on your computer.

A [quickstart](https://www.sphinx-doc.org/en/master/usage/quickstart.html) option is available in Sphinx for quick setting up of a new documentation, which is already implemented in Dyssol documentation.

You should also have [Anaconda](https://docs.anaconda.com/anaconda/install/) installed on your computer, which you will use for compling your documentation codes into web pages.


Edit and compile
----------------

In the python [configuration](https://www.sphinx-doc.org/en/master/usage/configuration.html?highlight=conf#module-conf) file `conf.py` located in folder `source`, you can adjust different properties for the documentation such as theme, page layout and extensions for rendering of special formats (like mathematical formula).

The `.rst` file with reStructuredText markup language is applied in Sphinx for editing the pages. For more information about this languague please refer to the [Markdown-Cheatsheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet#links).

For editing a document, open the corresponding `.rst` file in an editor and edit as you wish, save it and open Anaconda Prompt command window, get to the path where the documentation project locates, then run `make html` and you will see the compiling progress. Afterwards you can check your changes in web pages locatting in folder `build`.

