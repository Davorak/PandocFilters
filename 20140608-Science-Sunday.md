% Quick Scriptable Markdown with Pandoc
% Patrick Wheeler
% June 8 2014

# Swiss Arm Knife Converstion Tool

* Pandoc is can be used to convert between a number of markdown and markup file
formats

        Input formats:  docbook, haddock, html, json, latex, markdown,
                        markdown_github, markdown_mmd,
                        markdown_phpextra, markdown_strict, mediawiki,
                        native, opml, org, rst, textile
        Output formats: asciidoc, beamer, context, docbook, docx,
                        dzslides, epub, epub3, fb2, html, html5, icml,
                        json, latex, man, markdown, markdown_github,
                        markdown_mmd, markdown_phpextra,
                        markdown_strict, mediawiki, native, odt,
                        opendocument, opml, org, pdf*, plain, revealjs,
                        rst, rtf, s5, slideous, slidy, texinfo, textile
                        [*for pdf output, use latex or beamer and -o FILENAME.pdf]

# Focus

- markdown
- html
- A touch of json
- Easy of scripting
- Pandoc was used to make these slides


# markdown to html

```{ cmdBlock="pandoc -t html5" inClasses="markdown" outClasses="html"}
## hi

This is text and [this is a link.](http://www.example.com)
```

When rendered:

## hi

This is text and [this is a link.](http://www.example.com)


# More

```{ cmdBlock="pandoc -t html5" inClasses="markdown" outClasses="html"}
## This is a list

* item one
* item two
```

rendered:

## This is a list

* item one
* item two


# Easy of use

- Less verbose then html, json, man, opml, epub, etc.
    * Write in what you know convert to what you do not.
    * Still recoment markdown ubiquity.
- Flexible and scriptible
- Who has the time to learn all of these formats? Epessially if you want
content in multiple formats.


# Who has the time? markdown -> html

```{ cmdBlock="pandoc -t html" inClasses="markdown" outClasses="markdown"}
## This is a list

* item one
* item two
```


# Who has the time? markdown -> asciidoc

```{ cmdBlock="pandoc -t asciidoc" inClasses="markdown" outClasses="asciidoc"}
## This is a list

* item one
* item two
```


# Who has the time? markdown -> docbook

```{ cmdBlock="pandoc -t docbook" inClasses="markdown" outClasses="docbook"}
## This is a list

* item one
* item two
```


# Who has the time? markdown -> latex

```{ cmdBlock="pandoc -t latex" inClasses="markdown" outClasses="latex"}
## This is a list

* item one [link](http://www.example.com]
* item two
```


# Who has the time? markdown -> json

```{ cmdBlock="pandoc -t json" inClasses="markdown" outClasses="json"}
## This is a list

* item one
* item two
```

This can represent all of pandoc's internal AST.

# Who has the time? markdown -> etc


    Output formats: asciidoc, beamer, context, docbook, docx,
                    dzslides, epub, epub3, fb2, html, html5, icml,
                    json, latex, man, markdown, markdown_github,
                    markdown_mmd, markdown_phpextra,
                    markdown_strict, mediawiki, native, odt,
                    opendocument, opml, org, pdf*, plain, revealjs,
                    rst, rtf, s5, slideous, slidy, texinfo, textile
                    [*for pdf output, use latex or beamer and -o FILENAME.pdf]


# Scriptablity

Since it is easy to out the internal AST it is simple to filter and edit the AST
and send it along to other formats.

In fact that is what I have been doing for all of my little converstion
examples. I would have been too lazy to copy and paste all of the converstions
you have seen so far.

So instead I made a small haskell script to access and filter pandoc AST with
command line commands.


# Scriptablity CLI

```{ .markdown }
 ```{ cmdBlock="pandoc -t html5" inClasses="markdown" outClasses="html5"}
 ## This is a h2 header
 ```
```

becomes:

```{ cmdBlock="pandoc -t html5" inClasses="markdown" outClasses="html5"}
## This is a h2 header
```

# Scriptablity CLI - ls

```{ .markdown }
 ```{ showCmdBlock="ls -l" outClasses="bashOut"}
 ## This is a h2 header
 ```
```

becomes:

```{ showCmdBlock="ls -l" outClasses="bashOut"}
```

# Filters

- Pattern match on datatype
    * Paragraph
    * CodeBlock
    * BlockQuote
    * Ordered List
    * [etc, more at hackage page](http://hackage.haskell.org/package/pandoc-types-1.12.3.3/docs/Text-Pandoc-Definition.html#t:Block)
- Then transform and preform IO actions to create a new Block datatype.


# Command Line Filter

To inport a file with highlighting:
```{ .markdown }
 ```{ cmdBlock="cat pandocCmdFilter.hs" outClasses="haskell"}
 ```
```

```{ cmdBlock="cat pandocCmdFilter.hs" outClasses="haskell"}
```

# GraphViz - Inline dot files

```{ .showDot outFile="dotExample.png" }
digraph test {
    initState [label="0"]
    evens     [label="evens=[2,4..10]"]
    odds      [label="odds=[1,3..13]"]
    plus      [label="plus = evens + odds"]
    minus     [label="minus = evens - odds"]
    all       [label="all = [1..]"]
    a         [label="a = plus + minus"]
    b         [label="b = a + feedback"]
    f         [label="f = b * all"]
    fin       [label="fin = (f, a)"]

    evens -> plus
    odds  -> plus
    evens -> minus
    odds  -> minus
    all   -> f
    plus  -> a
    minus  -> a

    initState -> b [style=dotted,label="Initial State"]

    a -> b
    f -> b    [style=dotted,label="State Feedback"];
    b -> f
    f -> fin
    a -> fin
}
```

# Inline Diagrams Code

```{ .diagram .haskell outFile="./diaExample.svg" }
{-# LANGUAGE NoMonomorphismRestriction #-}
import Diagrams.Prelude
import Diagrams.TwoD
import Diagrams.Backend.SVG
import Data.List

example = hrule (2 * sum sizes) === circles # centerX
  where circles = hcat . map alignT . zipWith scale sizes
                $ repeat (circle 1)
        sizes   = [2,5,4,7,1,3]

main = renderSVG "diaExample.svg" (Width 500)  (example # lw 0.2)
```


# Easy of use + Scriptablity

- Pleanty of examples of scriptin Pandoc in:
    * Haskell
    * Python
    * Perl
    * others
    * If you can handle JSON you can script Pandoc
- Go forth and create and customize.
