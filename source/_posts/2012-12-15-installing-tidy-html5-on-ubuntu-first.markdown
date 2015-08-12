---
layout: post
title: "Installing tidy-html5 on Ubuntu, first step to get valid & structured HTML5"
date: 2012-12-15
comments: true
categories:
 - html5
 - ubuntu
 - tidy
---

At my work ([OERPUB](http://oerpub.org/)) we are starting to transform
any HTML and especially HTML5 content to a structured HTML5 for further
processing into various output formats (html5, epub, pdf, xml).
<!-- more -->
The problem with transforming HTML5 to a more structured HTML5 is the
parsing. The way to make this transformation is currently XSLT and we
need XML compatible (X)HTML5 for this.

In the past we used [HTML tidy](http://tidy.sourceforge.net/) which
handles all HTML soup and transforms it to valid XHTML. The only problem
is that it is dated, the last version is from **2008** and it does
**not** support HTML5 (and we also want MathML support).

After some searching we've found
http://w3c.github.com/tidy-html5 which is a fork of tidy with support
of HTML5. After my quick tests it seems compatible with the old tidy and
(more important for me) also compatible to
[pytidylib](http://countergram.com/open-source/pytidylib) so that I can
still use my old python code but with new HTML5 tidy options. :)

Here are instructions on how to replace the old HTML tidy which is
included in Ubuntu (tested on 10.04 & 12.04) with the new HTML5 tidy:

Remove all old tidy implementations
```bash
sudo apt-get remove libtidy-0.99-0 tidy
```

Get git, libtool and automake if you do not have them already
```bash
sudo apt-get install git-core automake libtool
```
Clone tidy-html5 repository in a directory of your choice
```bash
git clone https://github.com/w3c/tidy-html5
cd tidy-html5
```

Building the libtidy shared library and install libtidy and tidy program ([copied from here](http://building%20the%20libtidy%20shared%20library/))
```bash
sh build/gnuauto/setup.sh && ./configure && make
sudo make install
```

So tidy and libtidy are now installed but Ubuntu will not find
libtidy by default because libtidy installed to the folder
`/usr/local/lib` which is normally not searched for
runtime libraries. So we have to edit ldconfig's search folders.
Open (with root/sudo rights) the file "/etc/ld.so.conf". Example:
```bash
gksu gedit /etc/ld.so.conf
```

and add this line to the file `/etc/ld.so.conf`
```
/usr/local/lib
```

Finally restart ldconfig and you are set!
```bash
sudo ldconfig
```

Just a side note:  
To remove HTML5 tidy and install old tidy again just go to its cloned
directory and type

```bash
sudo make uninstall
sudo apt-get install tidy libtidy-0.99-0
```

