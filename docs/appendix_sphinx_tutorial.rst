****************************************
附录：如何使用 Sphinx-doc 写书？
****************************************

1. 简介
----------------------------------------

当前这本书就是用 `Sphinx-doc(官方网站) <http://www.sphinx-doc.org>`_ 写的，采用的是 `readthedocs.org <https://readthedocs.org>`_ 提供的模板，`Sphinx-doc` 最初是用来创建 Python 文档的工具，现在被广泛用作科学技术写作工具。它支持各种版本的输出：HTML，LaTeX，ePub，PDF 等等；它可以很方便地制作交互引用（cross-reference; 不仅在当前文档，也包括网上其它用 `Sphinx-doc` 制作的文档）；可以自动生成索引；加上相应插件之后，会有更多功能，比如，将程序代码执行结果输出到文档…… 越来越多的软件开发者在使用 `Sphinx-doc` 编写自己所写软件的文档 —— 有兴趣的话，你现在就可以去 `readthedocs.org <https://readthedocs.org>`_ 看看……

使用 `Sphinx-doc` 写教程实在是太适合不过了…… 比如，它支持  LaTeX，所以可以轻松写出很复杂的公式：

.. math::

  W^{3\beta}_{\delta_1 \rho_1 \sigma_2} = U^{3\beta}_{\delta_1 \rho_1} + \frac{1}{8 \pi 2} \int^{\alpha_2}_{\alpha_2} d \alpha^\prime_2 \left[\frac{ U^{2\beta}_{\delta_1 \rho_1} - \alpha^\prime_2U^{1\beta}_{\rho_1 \sigma_2} }{U^{0\beta}_{\rho_1 \sigma_2}}\right]

尤其是，稍加配置之后，它可以帮作者把程序的输出结果直接镶进文本 —— 仅此一点就已经太好不过了！比如：

.. code:: bash

		.. command-output:: python -V

会被显示为：

.. command-output:: python -V

再比如，`Sphinx-doc` 创建的文档也可以从程序文件中获得结果：

.. code:: bash

    .. plot:: code/contour3d_demo.py

会被显示为：

.. plot:: code/contour3d_demo.py
   :include-source:

`Sphinx-doc` 使用 `RestructuredText <http://docutils.sourceforge.net/docs/ref/rst/restructuredtext.html>`_  (RST) 格式文本（相当于稍微复杂一点的 Markdown），RestructuredText 与 Markdown 类似，也是一种格式化文档标记系统，只不过，它稍微更复杂一点点，能够完成的功能却多出不少，学起来也不是很难。

----------------------------------------

2. 安装
----------------------------------------

安装 `Sphinx-doc` 之前，最好已经安装过 Python 的 Anaconda 发行版（Python 3.6 版本，2017）。Anaconda 会把很多相关的库都安装好，很省心。而后就简单了，打开 Terminal 执行以下命令即可：

.. code:: bash

  	$ pip install Sphinx

3. 创建文库
----------------------------------------

创建一个工作目录，而后，执行以下命令：

.. code:: bash

  	$ sphinx-quickstart

一路按照默认安装即可，反正，随后可以通过 `source` 目录中的 `conf.py` 修改其中的设置。安装完成之后，就可以在项目工作目录中执行以下命令，编译文档：

.. code:: bash
    
    $ make html

随后，可以用浏览器打开 `build` 目录中的 HTML 文档查看编译结果……

另外，若是希望将来能把项目导入 readthedocs.org，那么，`source` 目录应该命名为 `docs` —— readthedocs.org 会在你的 github 项目中寻找 `docs` 目录而后导入其中的 rst 及其相关文件文件。而是想要更改已经创建的项目，那么需要修改 `Makefile`：

.. code-block::  bash
	:emphasize-lines: 5

	# You can set these variables from the command line.
	SPHINXOPTS    =
	SPHINXBUILD   = python -msphinx
	SPHINXPROJ    = sphinx
	SOURCEDIR     = docs
	BUILDDIR      = build

4. 自动编译
----------------------------------------

每次手动刷新浏览器查看编译结果还是挺麻烦的…… 有个自动编译插件可以安装：

.. code:: bash
    
    $ pip install sphinx-autobuild

而后，在项目跟目录下打开 Terminal，执行以下命令：

.. code:: bash
    
  	$ sphinx-autobuild --open-browser <source-dir> <build-dir>

为了提高效率，我在用户根目录下的 `.bash_profile` 文件中加了一行：

.. code:: bash
		
	alias sab='sphinx-autobuild --open-browser source build'

以后只需要在 Terminal 中输入 ``sab ⏎`` 即可。此后，每次在编辑器中保存 `rst` 文件的时候，这个插件就会执行自动编译，而后刷新浏览器…… 相当方便。

5. 安装模板
--------------------------------------------

为了可以将来自定义模板，最好把模板安装在项目根目录中的 `/source/_themes/` 目录下（没有的话就创建一个）。比如，当前项目所使用的模板，是 `readdoc` 的模板，字体大小稍加修正。

.. code:: bash
	
		git@github.com:xiaolai/sphinx_rtd_theme.git

而后将其中的 `sphinx_rtd_theme` 目录拷贝到 `/source/_themes/` 之中。

在 `conf.py` 里添加以下内容：

.. code:: json

    html_theme = "sphinx_rtd_theme"
    html_theme_path = ["_themes", ]


6. SublimeText 编辑器
----------------------------------------

编辑 ReStructuredText，我选择的是 SublimeText，在我看来相对是最方便的。有相应的插件可供使用，自己也可以很方便地设置很多必要的 Snippets，提高输入编辑效率。

可以在 SublimeText 中安装以下两个插件：

.. epigraph::

	1. Restructured Text (RST) Snippets
	2. RestructuredText Improved

其中，Restructured Text (RST) Snippets 插件里的 auto completion 有一些需要补充的地方，也有一些需要修改的地方，比如，它在转换 ``h1`` 标记的时候，由于标记字符是按照英文字母计算的，所以经常会在编译的时候被警告很多次：

 ``...(WARNING/2) Title underline too short.`` 

所以，还不如直接加上一个四十个字符长度的下划线呢……


snippets 的补充


7. 学习 RestructuredText 的最好方法
-------------------------------------------

下载 `Sphinx-doc` 的文档，随时在里面查找实际的例子……

.. code:: bash

    git clone https://github.com/sphinx-doc/sphinx.git

8. 在 Sphinx-doc 项目里同时使用 MD 和 RST
----------------------------------------------------------

安装 recommonmark 工具包

.. code:: bash

	$ pip install recommonmark

若后在 `conf.py` 中添加:

.. code:: json

	from recommonmark.parser import CommonMarkParser

	source_parsers = {
	    '.md': CommonMarkParser,
	}

	source_suffix = ['.rst', '.md']


----------------------------------------

插入程序输出结果

显示 matplotlib 结果

.. code:: json

	extensions = ['sphinx.ext.autodoc',
	    'sphinx.ext.doctest',
	    'sphinx.ext.intersphinx',
	    'sphinx.ext.todo',
	    'sphinx.ext.coverage',
	    'sphinx.ext.mathjax',
	    'sphinx.ext.ifconfig',
	    'sphinx.ext.viewcode',
	    'sphinx.ext.githubpages',
	    'sphinx.ext.graphviz',
	    'matplotlib.sphinxext.only_directives',
	    'matplotlib.sphinxext.plot_directive',
	    'IPython.sphinxext.ipython_directive',
	    'IPython.sphinxext.ipython_console_highlighting',
	    'sphinx.ext.inheritance_diagram',
	    'numpydoc',
	    'sphinxcontrib.programoutput']





  pip install sphinxcontrib-plantuml