# How I’m a Productive Programmer With a Memory of a Fruit Fly
[How I’m a Productive Programmer With a Memory of a Fruit Fly](https://hynek.me/articles/productive-fruit-fly-programmer/) 

 A love letter to tools that changed everything for me.

Programming Over the Years
--------------------------

Programming got vastly more varied compared to when I started dabbling in [_AmigaBASIC_](https://en.wikipedia.org/wiki/AmigaBASIC) in the mid-1990s. Back then you could buy one very big book about the computer you’re programming and were 99% there. That book, full with earmarks and Post-its, lay next to you while hacking into your monochrome editor, always in reach.

Nowadays the book on your frontend web framework can be thicker than what a C64 programmer needed to write a complete game. On the other hand, the information for everything that we need to write code _today_ is usually no more than one click away.

Nobody can imagine paying for developer documentation anymore – both [Microsoft](https://learn.microsoft.com/en-us/windows/apps/desktop/) and [Apple](https://developer.apple.com/documentation/) offer their documentation on the web for free for everyone. And don’t get me even started about open-source projects!

In times of [_npm_](https://www.npmjs.com/), [_PyPI_](https://pypi.org/), and GitHub, it’s hard to explain that requiring anything beyond what your operating system offers, used to be a controversial decision that had to be weighted judiciously. Often, you shipped your dependencies along with your product[1](#fn:1).

* * *

The new availability is great and variety is healthy, but it leads to **fragmentation of the information that you need to be productive**.

People have dozens of tabs open[2](#fn:2) with documentation for the packages they’re using at the moment, hectically switching between them to find the right one. As someone who has worked from the [best longboarding spot in the world](https://en.wikipedia.org/wiki/Muizenberg) where several 10,000s of people share _one_ [POP](https://en.wikipedia.org/wiki/Point_of_presence), I can tell you that online-only docs aren’t only a problem when your Internet dies altogether. Especially an online search function with flaky Internet is worse than no search function.

If you’re like me and are a polyglot working with multiple programming languages with enormous sub-communities each (even within Python, _Flask_ + _SQLAlchemy_ + _Postgres_ is a very different beast from writing _asyncio_\-based network servers), it hurts my head to even imagine one could remember the arguments of every method I’m using. Primarily, if you’re _really_ like me and barely remember your own phone number.

That’s why it was such a life-changing event for me when I found [_Dash_](https://kapeli.com/dash) in 2012.

API Documentation Browsers
--------------------------

> Your mind is for having ideas, not holding them.

[![](https://hynek.me/articles/productive-fruit-fly-programmer/dash-search.png)
](https://hynek.me/articles/productive-fruit-fly-programmer/dash-search.original.png)

_Dash_ searching

_Dash_ gives me the superpower of having all relevant APIs one key press away:

*   I press ⌥Space and a floating window pops up with an _activated_ search bar,
*   I start typing the rough name of the API or topic,
*   I choose from the suggestions and land on the symbol within the official _project documentation_,
*   I press Escape, the floating window disappears and I can start typing code immediately because my editor is in focus again.
*   If I forget what I just read, I press ⌥Space again and the window pops up at the same position.

All this is _blazing fast_ – I want to make such a round trip in under 2 seconds. It _has_ to be _that_ quick, so my brain doesn’t lose track of what I was doing. At this point, I can do it subconsciously. It’s the forgotten bliss of native applications – yes I know about [‌https://devdocs.io](https://devdocs.io/).

**Having _all_ API docs one key press away is profoundly empowering.**

The less energy I spend trying to remember the argument of a function or the import path of a class, the more energy I can spend on thinking about the problem I’m solving.

I don’t consider myself particularly smart, so I take any opportunity to lower my mental load.

While _Dash_ is a $30 Mac app (also available as part of [_Setapp_](https://setapp.com/) subscriptions!), there’s the **free Windows** and **Linux** version called [_Zeal_](https://zealdocs.org/), and a $20 **Windows** app called [_Velocity_](https://velocity.silverlakesoftware.com/). _Of course_, there’s also at least one **Emacs** package doing the same thing: [_helm-dash_](https://github.com/dash-docs-el/helm-dash).

Meaning: you can have this API bliss on any platform! In the following, I’ll only write about _Dash_, because that’s what I’m using, but unless noted otherwise, it applies to all of them.

The one thing they have in common is the format of the local documentation.

Documentation Sets
------------------

They all use Apple’s [_Documentation Set Bundles_](https://developer.apple.com/library/archive/documentation/DeveloperTools/Conceptual/Documentation_Sets/010-Overview_of_Documentation_Sets/docset_overview.html#//apple_ref/doc/uid/TP40005266-CH13-SW6) (_docsets_) which are directories with the HTML documentation, metadata in an XML-based [_property list_](https://en.wikipedia.org/wiki/Property_list), and a search index in a [_SQLite_](https://sqlite.org/) database:

```
some.docset └── Contents  ├── Info.plist # ← metadata └── Resources ├── Documents # ← root dir of HTML docs │   └── index.html └── docSet.dsidx  # ← SQLite db w/ search index 
```

If you have a bunch of HTML files on your disk, you can convert them into a _docset_ that can be consumed by _Dash_. It’s just HTML files with metadata. And since it’s HTML files on **your disk**, all this works **offline**.

Therefore, _docsets_ can replace documentation that you already keep locally on your computer for faster and/or offline access without doing anything special. Just package it up into the necessary directory structure, [add an empty index](https://kapeli.com/docsets#createsqlite), and fill out simple metadata.

_Shazam_! Now you can conjure them with a single keypress and get rid of them with another.

* * *

Let’s circle back to the boring history lesson from the beginning: there’s a myriad of projects that I use across countless platforms – every day. And I’m not talking just about programming APIs here: _Ansible_ roles, CSS classes, _HAProxy_ configuration, _Postgres_ (and SQL!) peculiarities…**it’s a lot**.

[![](https://hynek.me/articles/productive-fruit-fly-programmer/dash-docsets.png)
](https://hynek.me/articles/productive-fruit-fly-programmer/dash-docsets.original.png)

Installed _Dash_ _docsets_

And while Python and Go core documentation ship with _Dash_, and while [_Godoc_](https://go.dev/blog/godoc) documentation can be added directly by URL[3](#fn:3), no matter how hard _Dash_ will try: in the fragmented world of modern software development, it will never be able to deliver everything I need.

### Sphinx

The biggest gap for me is [_Sphinx_](https://www.sphinx-doc.org/)\-based docs that dominate (not only) the Python ecosystem.

_Sphinx_ is a language-agnostic framework to write documentation. Not just API docs or just narrative docs: **all of it**, with rich interlinking. It used to be infamous for forcing [_reStructuredText_](https://en.wikipedia.org/wiki/ReStructuredText) on its users, but nowadays more and more projects use the wonderful [_MyST_ package](https://myst-parser.readthedocs.io/) to do it in _Markdown_. If you have any preconceptions about the look of _Sphinx_ documentation, I urge you to visit the _[Sphinx Themes Gallery](https://sphinx-themes.org/)_ and see how pretty your docs can be. It’s written in Python, but it’s used widely, including the [Linux kernel](https://github.com/torvalds/linux/tree/master/Documentation), Apple’s [_Swift_](https://github.com/apple/swift/tree/main/docs), the [_LLVM_ (_Clang_!) project](https://github.com/llvm/llvm-project/tree/main/clang/docs), or wildly popular [PHP projects](https://github.com/guzzle/guzzle/tree/master/docs).

And it offers the exact missing piece: an index for API entries, sections, glossary terms, configuration options, command line arguments, and more – all distributed throughout your documentation any way you like, but always mutually _linkable_. I find this wonderful particularly if you follow a systematic framework like [_‌Diátaxis_](https://diataxis.fr/).

The key component that makes this possible is technically just an extension: [_intersphinx_](https://www.sphinx-doc.org/en/master/usage/extensions/intersphinx.html). Originally made for inter-project linking (hence the name) it offers a machine-readable index for us to take. That index grew so popular that it’s now supported by the [_MkDocs_](https://www.mkdocs.org/) extension [_mkdocstrings_](https://mkdocstrings.github.io/) and [_pydoctor_](https://github.com/twisted/pydoctor). You can recognize _intersphinx_\-compatible documentation exactly by that index file: `objects.inv`.

And that’s why, 10 years ago almost to the day, I started the _doc2dash_ project.

doc2dash
--------

[_doc2dash_](https://github.com/hynek/doc2dash) is a command line tool that you can get from my [_Homebrew_ tap](https://github.com/hynek/homebrew-tap), download one of the [pre-built binaries](https://hynek.me/til/python-portable-binaries/) for Linux, macOS, and Windows from its [release page](https://github.com/hynek/doc2dash/releases), or install from [PyPI](https://pypi.org/project/doc2dash/).

Then, all you have to do is to point it at a directory with _intersphinx_\-compatible documentation and it will do everything necessary to give you a _docset_.

[![](https://hynek.me/articles/productive-fruit-fly-programmer/doc2dash.png)
](https://hynek.me/articles/productive-fruit-fly-programmer/doc2dash.original.png)

_doc2dash_ converting

Please note that the name is _**doc**2dash_ and not _sphinx2dash_. It was always meant as a _framework_ for writing high-quality converters, the first ones being _Sphinx_ and _pydoctor_. That hope sadly didn’t work out, because – understandably – every community wanted to use their own language and tools.

Those tools usually look quite one-off to me though, so I’d like to re-emphasize that I would _love_ to work with others adding support for other documentation formats. Don’t reinvent the wheel, the framework is all there! [It’s just a bunch of lines of code](https://doc2dash.readthedocs.io/en/stable/extending/)! You don’t even have to share your parser with me and the world.

* * *

The fact that both _Dash_ and _doc2dash_ have existed well over a decade and I _still_ see friends have a bazillion tabs with API docs open has been positively heartbreaking for me. I keep showing people _Dash_ in action and they keep saying it’s cool and put it on their _someday_ list. Barring another nudge, _someday_ never comes.

While the fruit-fly part of this article ends here, let me try to give you that nudge with a step-by-step how-to guide such that _today_ becomes _someday_!

How To Convert and Submit Your Docs
-----------------------------------

The goal of this how-to is to teach you how to convert _intersphinx_\-compatible documentation to a _docset_ and how to submit it to _Dash_’s [_user-generated docset registry_](https://github.com/Kapeli/Dash-User-Contributions), such that others don’t have to duplicate your work.

I’ll assume you have picked and installed your API browser of choice. It doesn’t matter which one you use, but this how-to guide uses _Dash_. For _optionally_ submitting the _docset_ at the end, you’ll also need a basic understanding of GitHub and its [pull request workflow](https://docs.github.com/en/get-started/quickstart/contributing-to-projects).

**I** will be using this how-to as an occasion to _finally_ start publishing _docsets_ of my own projects, starting with [_structlog_](https://www.structlog.org/). I suggest **you** pick an _intersphinx_\-compatible project that isn’t supported by _Dash_ yet and whose documentation’s tab you visit most often.

Let’s do this!

### Getting doc2dash

If you’re already using [_Homebrew_](https://brew.sh/), the easiest way to get _doc2dash_ is to use my [tap](https://github.com/hynek/homebrew-tap/):

```
$ brew install hynek/tap/doc2dash 
```

There are pre-built [_bottles_](https://docs.brew.sh/Bottles) for Linux _x86-64_ and macOS on both _x86-64_ and _Apple silicon_, so the installation should be very fast.

Unless you know your way around Python packaging, the next best way is pre-built binaries from the [release page](https://github.com/hynek/doc2dash/releases/latest). Currently, it offers binaries for Linux, Windows, and macOS – all on _x86-64_. I hope to offer more in the future if this proves to be popular.

Finally, you can get it from [PyPI](https://pypi.org/project/doc2dash/). I _strongly_ recommend using [_pipx_](https://pypa.github.io/pipx/) and the easiest way to run _doc2dash_ with it is:

```
$ pipx run doc2dash --help 
```

### Building Documentation

Next comes the biggest problem and source of frequent feature requests for _doc2dash_: you need the documentation in a complete, built form. Usually, that means that you have to download the repository and figure out how to build the docs before even installing _doc2dash_ because most documentation sites unfortunately don’t offer a download of the whole thing.

My heuristic is to look for a `tox.ini` or `noxfile.py` first and see if it builds the documentation. If it doesn’t, I look for a [`readthedocs.yml`](https://dev.readthedocs.io/en/latest/design/yaml-file.html), and if even that lets me down, I’m on the lookout for files named like `docs-requirements.txt` or optional installation targets like `docs`. My final hope is to go through pages of YAML and inspect CI configurations.

Once you’ve managed to install all dependencies, it’s usually just a matter of `make html` in the documentation directory.

* * *

After figuring this out, you should have a directory called `_build/html` for _Sphinx_ or `site` for _MkDocs_.

Please note with _MkDocs_ that if the project doesn’t use the [_mkdocstrings_](https://mkdocstrings.github.io/) extension – which alas, right now is virtually all of the popular ones – there won’t be an `objects.inv` file and therefore no API data to be consumed.

I truly hope that more _MkDocs_\-based projects add support for _mkdocstrings_ in the future! As with _Sphinx_, it’s language-agnostic.

### Converting

Following the hardest step comes the easiest one: converting the documentation we’ve just built into a _docset_.

All you have to do is point _doc2dash_ at the directory with HTML documentation and wait:

That’s all!

_doc2dash_ knows how to extract the name from the _intersphinx_ index and uses it by default (you can override it with `--name`). You should be able to add this _docset_ to an API browser of your choice and everything should work.

* * *

If you pass `--add-to-dash` or `-a`, the final _docset_ is automatically added to _Dash_ when it’s done. If you pass `--add-to-global` or `-A`, it moves the finished docset to a global directory (`~/Library/Application Support/doc2dash/DocSets`) and adds it from there. I rarely run _doc2dash_ without `-A` when creating _docset_s for myself.

### Improving Your Documentation Set

_Dash_’s documentation has a [bunch of recommendations](https://kapeli.com/docsets#contributetodash) on how you can improve the _docset_ that we built in the previous step. It’s important to note that the next five steps are strictly optional and more often than not, I skip them because I’m lazy.

But in this case, I want to submit the _docset_ to _Dash_’s user-contributed registry, so let’s go the full distance!

#### Set the Main Page

With _Dash_, you can always search _all_ installed _docsets_, but sometimes you want to limit the scope of search. For example, when I type `p3:` (the colon is significant), _Dash_ switches to only searching Python 3 _docset_. Before you start typing, it offers you a menu underneath the search box whose first item is “Main Page”.

When converting _structlog_ docs, this main page is the index that can be useful, but usually not what I want. When I got to the main page, I want to browse the narrative documentation.

The _doc2dash_ option to set the main page is `--index-page` or `-I` and takes the file name of the page you want to use, relative to the documentation root.

Confusingly, the file name of the index is `genindex.html` and the file name of the main page is the HTML-typical `index.html`. Therefore, we’ll add `--index-page index.html` to the command line.

#### Add an Icon

Documentation sets can have icons that are shown throughout _Dash_ next to the _docsets_’s names and symbols. That’s pretty but also helpful to recognize _docsets_ faster and if you’re searching across multiple _docsets_, where a symbol is coming from.

_structlog_ has a cute beaver logo, so let’s use [_ImageMagick_](https://imagemagick.org/) to resize the logo to 16x16 pixels:

```
$ magick \  docs/_static/structlog_logo_transparent.png \ -resize 16x16 \ docs/_static/docset-icon.png 
```

Now we can add it to the _docset_ using the `--icon docset-icon.png` option.

#### Support Online Redirection

Offline docs are awesome, but sometimes it can be useful to jump to the online version of the documentation page you’re reading right now. A common reason is to peruse a newer or older version.

_Dash_ has the menu item “Open Online Page ⇧⌘B” for that, but it needs to know the base URL of the documentation. You can set that using `--online-redirect-url` or `-u`.

For Python packages on [_Read the Docs_](https://readthedocs.org/) you can pick between the `stable` (last VCS tag) or `latest` (current main branch).

I think `latest` makes more sense, if you leave the comfort of offline documentation, thus I’ll add:

```
--online-redirect-url https://www.structlog.org/en/latest/ 
```

#### Putting It All Together

We’re done! Let’s run the whole command line and see how it looks in _Dash_:

```
$ doc2dash \  --index-page index.html \ --icon docs/_static/docset-icon.png \ --online-redirect-url https://www.structlog.org/en/latest/ \ docs/_build/html Converting intersphinx docs from '/Users/hynek/FOSS/structlog/docs/_build/html' to 'structlog.docset'. Parsing documentation... Added 238 index entries. Patching for TOCs... ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 100% 0:00:00 
```

Wonderful:

[![](https://hynek.me/articles/productive-fruit-fly-programmer/structlog-docset.png)
](https://hynek.me/articles/productive-fruit-fly-programmer/structlog-docset.original.png)

_structlog_’s Main Page

Notice the icon in the search bar and pressing ⇧⌘B on any page with any anchor takes me to the same place in the latest version of the online docs.

#### Automation

Since I want to create a new version of the _docsets_ for every new release, the creation needs to be automated. _structlog_ is already using GitHub Actions as CI, so it makes sense to use it for building the docset too.

For local testing, I’ll take advantage of _doc2dash_ being a Python project and use a [_tox_](https://tox.wiki/) environment that reuses the dependencies that I use when testing documentation itself.

The environment installs `structlog[docs]` – i.e. the package with optional `docs` dependencies, plus _doc2dash_. Then it runs `commands` in order:

```
[testenv:docset] extras = docs deps = doc2dash allowlist_externals =  rm cp tar commands =  rm -rf structlog.docset docs/_build sphinx-build -n -T -W -b html -d {envtmpdir}/doctrees docs docs/_build/html doc2dash --index-page index.html --icon docs/_static/docset-icon.png --online-redirect-url https://www.structlog.org/en/latest/ docs/_build/html cp docs/_static/docset-icon@2x.png structlog.docset/icon@2x.png tar --exclude='.DS_Store' -cvzf structlog.tgz structlog.docset 
```

Now I can build a _docset_ just by calling `tox -e docset`. [Until _doc2dash_ supports hi-res icons](https://github.com/hynek/doc2dash/issues/130), it also copies a 32x32 pixels big version of the logo directly into the _docset_.

Doing that in CI is trivial, but entails tons of boilerplate, so I’ll just [link to the workflow](https://github.com/hynek/structlog/blob/main/.github/workflows/build-docset.yml). Note the `upload-artifact` action at the end that allows me to download the built _docset_s from the run summaries[4](#fn:4).

* * *

At this point, we have a _great_ _docset_ that’s built _automatically_. Time to share it with the world!

### Submitting

In the final step, we’ll submit our _docset_ to _Dash_’s user-contributed repository, so other people can download it comfortably from _Dash_’s GUI. Conveniently, _Dash_ uses a concept for the [whole process](https://github.com/Kapeli/Dash-User-Contributions#contribute-a-new-docset) that’s probably familiar to every open-source aficionado[5](#fn:5): GitHub pull requests.

The first step is checking the [_Docset Contribution Checklist_](https://github.com/Kapeli/Dash-User-Contributions/wiki/Docset-Contribution-Checklist). Fortunately we – or in some cases: _doc2dash_ – have already taken care of everything!

So let’s move right along, and fork the [https://github.com/Kapeli/Dash-User-Contributions](https://github.com/Kapeli/Dash-User-Contributions) repo and clone it to your computer.

First, you have to copy the `Sample_Docset` directory into `docsets` and rename it while doing so. Thus, the command line is for me:

```
$ cp -a Sample_Docset docsets/structlog 
```

Let’s enter the directory with `cd docsets/structlog` and take it from there further.

The main step is adding the _docset_ itself – but as a _gzipped_ _tar_ file. The contribution guide even gives us the template for creating it. In my case the command line is:

```
$ tar --exclude='.DS_Store' -cvzf structlog.tgz structlog.docset 
```

You may have noticed that I’ve already done the _tar_\-ing in my _tox_ file, so I just have to copy it over:

```
$ cp ~/FOSS/structlog/structlog.tgz . 
```

It also wants the icons _additionally_ to what is in the _docset_, so I copy them from the docset:

```
$ cp ~/FOSS/structlog/structlog.docset/icon* . 
```

Next, it would like us to fill in metadata in the `docset.html` file which is straightforward in my case:

```
{  "name": "structlog", "version": "22.1.0", "archive": "structlog.tgz", "author": { "name": "Hynek Schlawack", "link": "https://github.com/hynek" }, "aliases": [] } 
```

Finally, it wants us to write some documentation about who we are and how to build the _docset_. After looking at other [examples](https://github.com/Kapeli/Dash-User-Contributions/tree/master/docsets), I’ve settled on the following:

```
# structlog   <https://www.structlog.org/>   Maintained by [Hynek Schlawack](https://github.com/hynek/).     ## Building the Docset   ### Requirements   - Python 3.10 - [*tox*](https://tox.wiki/)     ### Building   1. Clone the [*structlog* repository](https://github.com/hynek/structlog). 2. Check out the tag you want to build. 3. `tox -e docset` will build the documentation and convert it into `structlog.docset` in one step. 
```

The _tox_ trick is paying off – I don’t have to explain Python packaging to anyone!

Don’t forget to delete stuff from the sample _docset_ that we don’t use:

```
$ rm -r versions Sample_Docset.tgz 
```

* * *

We’re done! Let’s check in our changes:

```
$ git checkout -b structlog $ git add docsets/structlog $ git commit -m "Add structlog docset" [structlog 33478f9] Add structlog docset  5 files changed, 30 insertions(+) create mode 100644 docsets/structlog/README.md create mode 100644 docsets/structlog/docset.json create mode 100644 docsets/structlog/icon.png create mode 100644 docsets/structlog/icon@2x.png create mode 100644 docsets/structlog/structlog.tgz $ git push -u 
```

Looking good – time for a [pull request](https://github.com/Kapeli/Dash-User-Contributions/pull/3891)!

A few hours later:

[![](https://hynek.me/articles/productive-fruit-fly-programmer/dash-contributed.png)
](https://hynek.me/articles/productive-fruit-fly-programmer/dash-contributed.original.png)

Our contributed _structlog_ _docset_ inside _Dash_!

Big success: everyone can download the _structlog_ _Documentation Set_ now which concludes our little how-to!

Closing
-------

I hope I have both piqued your interest in API documentation browsers and demystified the creation of your own documentation sets. My goal is to turbocharge programmers who – like me – are overwhelmed by all the packages they have to keep in mind while getting stuff done.

My biggest hope, though, is that this article inspires someone to help me add more formats to _doc2dash_, such that even more programmers get to enjoy the bliss of API documentation at their fingertips.

Another hope I’ve developed after publishing this article is that [_Zeal_](https://github.com/zealdocs/zeal/) gets revitalized. Reportedly, it’s a bit long on the tooth with the last release dating back [four years](https://github.com/zealdocs/zeal/releases/tag/v0.6.1). Since it’s an open-source project, it would be cool if it could get a few new hands and we could have good API browsers on all platforms.

* * *

I’ve done a terrible job at promoting _doc2dash_ in the past decade and I hope the next ten years will go better!

Since I’ve been accused of this article being an ad: I have no business relationship with the author of _Dash_ and he didn’t know I’m writing this article until I showed him a late draft for fact-checking.

Sometimes people like products that cost money and it should be possible to write about them if they’ve been transformative to one’s professional life. _Especially_, if it’s coming from an indie developer.