---
title: Use Vim like an IDE
id: 511
categories:
  - 工具
  - 转载
date: 2012-07-31 12:18:08
tags:
---

刚在新浪微博上面看到的，转下来留个记号，

原文链接[http://vim.wikia.com/wiki/Use_Vim_like_an_IDE](http://vim.wikia.com/wiki/Use_Vim_like_an_IDE)

I use Vim for all text editing, even software development. At one point I stopped using IDEs. One major reason is that Vim can do all the major things I need from IDEs (tabs, file trees, greping, syntax highlighting, indentation, completion, "quickfix"ing, etc).
<table id="toc">
<tbody>
<tr>
<td>
<div id="toctitle">

## Contents

</div></td>
</tr>
</tbody>
</table>

## Vim Plugins[![](data:image/gif;base64,R0lGODlhAQABAIABAAAAAP///yH5BAEAAAEALAAAAAABAAEAQAICTAEAOw%3D%3D)](http://vim.wikia.com/wiki/Use_Vim_like_an_IDE?action=edit&amp;section=1 "Edit Vim Plugins section")

Still Vim needs plugins to do some IDE-like things that aren't built in. Here are some Vim scripts that make Vim more like an IDE.

Note: You can use [pathogen](http://www.vim.org/scripts/script.php?script_id=2332) to isolate your plugins and make it easier to experiment with new plugins.

### Project/Filetree Browsing

*   [NERDTree](http://www.vim.org/scripts/script.php?script_id=1658) is a tree explorer plugin for navigating the filesystem.
*   [vtreeexplorer](http://www.vim.org/scripts/script.php?script_id=184) is a tree based file explorer.
*   [project](http://www.vim.org/scripts/script.php?script_id=69) gives you a "project" view of files, rather than a straight file system view

### Buffer/File Browsing[![](data:image/gif;base64,R0lGODlhAQABAIABAAAAAP///yH5BAEAAAEALAAAAAABAAEAQAICTAEAOw%3D%3D)](http://vim.wikia.com/wiki/Use_Vim_like_an_IDE?action=edit&amp;section=3 "Edit Buffer/File Browsing section")

*   [bufexplorer](http://www.vim.org/scripts/script.php?script_id=42) lets you navigate through open buffers
*   [minibufexpl](http://www.vim.org/scripts/script.php?script_id=159) Elegant buffer explorer; takes very little screen space.
*   [lookupfile](http://www.vim.org/scripts/script.php?script_id=1581) Lookup files using Vim7 ins-completion
*   [Command-T plugin](https://wincent.com/products/command-t/), inspired by the "Go to File" window bound to Command-T in TextMate
*   [MRU](http://www.vim.org/scripts/script.php?script_id=521) access recently opened files.

### Code Browsing[![](data:image/gif;base64,R0lGODlhAQABAIABAAAAAP///yH5BAEAAAEALAAAAAABAAEAQAICTAEAOw%3D%3D)](http://vim.wikia.com/wiki/Use_Vim_like_an_IDE?action=edit&amp;section=4 "Edit Code Browsing section")

*   [taglist](http://www.vim.org/scripts/script.php?script_id=273) gives you an outline of the source you're viewing
*   [Tagbar](http://www.vim.org/scripts/script.php?script_id=3465) similar to taglist but can order tags by scope. Recommend for programming languages with classes, e.g. C++, Java, Python.
*   [Indexer](http://www.vim.org/scripts/script.php?script_id=3221) generates tags for all files in project automatically and keeps tags up-to-date. Using ctags. Works well with project plugin or independently.
*   [CCTree](http://www.vim.org/scripts/script.php?script_id=2368) is a Call-Tree Explorer, Cscope based source-code browser, and code flow analyzer.
*   [exUtility](http://www.vim.org/scripts/script.php?script_id=1729) global search, symbol search, tag track...(Like IDE/Source Insight).
*   [ShowMarks](http://www.vim.org/scripts/script.php?script_id=152) visually shows the location of marks.
See also [Browsing programs with tags](http://vim.wikia.com/wiki/Browsing_programs_with_tags "Browsing programs with tags") and [Cscope](http://vim.wikia.com/wiki/Cscope "Cscope").

### Writing Code[![](data:image/gif;base64,R0lGODlhAQABAIABAAAAAP///yH5BAEAAAEALAAAAAABAAEAQAICTAEAOw%3D%3D)](http://vim.wikia.com/wiki/Use_Vim_like_an_IDE?action=edit&amp;section=5 "Edit Writing Code section")

*   A plethora of [code snippet/template plugins](http://vim.wikia.com/wiki/Category:Automated_Text_Insertion#Related_scripts "Category:Automated Text Insertion") are available, many offering [TextMate](http://en.wikipedia.org/wiki/Textmate#Snippets "wikipedia:Textmate")-like snippet features.
*   [AutoComplPop](http://www.vim.org/scripts/script.php?script_id=1879) gives you code completion as you type.
*   [CRefVim](http://www.vim.org/scripts/script.php?script_id=614) A C-reference manual especially designed for Vim.
See also [Omni completion](http://vim.wikia.com/wiki/Omni_completion "Omni completion") and [Make Vim completion popup menu work just like in an IDE](http://vim.wikia.com/wiki/Make_Vim_completion_popup_menu_work_just_like_in_an_IDE "Make Vim completion popup menu work just like in an IDE").

### Vim Functionality[![](data:image/gif;base64,R0lGODlhAQABAIABAAAAAP///yH5BAEAAAEALAAAAAABAAEAQAICTAEAOw%3D%3D)](http://vim.wikia.com/wiki/Use_Vim_like_an_IDE?action=edit&amp;section=6 "Edit Vim Functionality section")

*   [matchit](http://www.vim.org/scripts/script.php?script_id=39) improves % matching
*   [bufkill](http://www.vim.org/scripts/script.php?script_id=1147) allows you to delete a buffer without actually closing the window.
*   [gundo](http://www.vim.org/scripts/script.php?script_id=3304) visualizes your undo tree.
*   [surround](http://www.vim.org/scripts/script.php?script_id=1697) makes it easier to delete/change/add parentheses/quotes/XML-tags/much more.

### IDE integration[![](data:image/gif;base64,R0lGODlhAQABAIABAAAAAP///yH5BAEAAAEALAAAAAABAAEAQAICTAEAOw%3D%3D)](http://vim.wikia.com/wiki/Use_Vim_like_an_IDE?action=edit&amp;section=7 "Edit IDE integration section")

You may want to use your IDE for some tasks like debugging, so some integration between Vim and the IDE can be helpful.

*   [Integrate gvim with Visual Studio](http://vim.wikia.com/wiki/Integrate_gvim_with_Visual_Studio "Integrate gvim with Visual Studio")
*   [Eclim](http://eclim.org/) brings Eclipse functionality to the Vim editor.

### Source Control Integration[![](data:image/gif;base64,R0lGODlhAQABAIABAAAAAP///yH5BAEAAAEALAAAAAABAAEAQAICTAEAOw%3D%3D)](http://vim.wikia.com/wiki/Use_Vim_like_an_IDE?action=edit&amp;section=8 "Edit Source Control Integration section")

There are many Vim plugins for different source control management systems. Here are a few.

*   [vcscommand.vim](http://www.vim.org/scripts/script.php?script_id=90) - CVS/SVN/SVK/git/hg/bzr integration plugin
*   [fugitive](http://www.vim.org/scripts/script.php?script_id=2975) - git integration
*   [perforce](http://www.vim.org/scripts/script.php?script_id=240) - perforce integration
See also [Category:VersionControl](http://vim.wikia.com/wiki/Category:VersionControl)

## Debugging[![](data:image/gif;base64,R0lGODlhAQABAIABAAAAAP///yH5BAEAAAEALAAAAAABAAEAQAICTAEAOw%3D%3D)](http://vim.wikia.com/wiki/Use_Vim_like_an_IDE?action=edit&amp;section=9 "Edit Debugging section")

There are several projects to add debugging functionality to vim

*   [Clewn](http://clewn.sourceforge.net/) implements full gdb support in the vim editor: breakpoints, watch variables, gdb command completion, assembly windows, etc.
*   [vim-debug](http://jaredforsyth.com/projects/vim-debug/), which creates an integrated debugging environment in VIM.
*   [gdbvim](http://www.vim.org/scripts/script.php?script_id=84) plugin: Watch in vim what you debug in gdb. And more.

## Refactoring[![](data:image/gif;base64,R0lGODlhAQABAIABAAAAAP///yH5BAEAAAEALAAAAAABAAEAQAICTAEAOw%3D%3D)](http://vim.wikia.com/wiki/Use_Vim_like_an_IDE?action=edit&amp;section=10 "Edit Refactoring section")

*   [Vim as a refactoring tool and some examples in C sharp](http://vim.wikia.com/wiki/Vim_as_a_refactoring_tool_and_some_examples_in_C_sharp "Vim as a refactoring tool and some examples in C sharp")
*   [**refactor** plugin](http://www.vim.org/scripts/script.php?script_id=2087)
*   [**renamec** plugin](http://www.vim.org/scripts/script.php?script_id=2164)

## Comments[![](data:image/gif;base64,R0lGODlhAQABAIABAAAAAP///yH5BAEAAAEALAAAAAABAAEAQAICTAEAOw%3D%3D)](http://vim.wikia.com/wiki/Use_Vim_like_an_IDE?action=edit&amp;section=11 "Edit Comments section")

When using Visual Studio, see [ViEmu](http://www.ngedit.com/viemu.html).

* * *

Code navigation in vi offers much more than a standard IDE, because of the ability to execute the desired combination of commands. Generate an index much more rapidly than an IDE with a heavy GUI interface:

For example, one can take advantage of the tag stack:

For C++, follow the instructions: [on using OmniCpp](http://design.liberta.co.za/articles/code-completion-intellisense-for-cpp-in-vim-with-omnicppcomplete/) Define a custom .ctags file

<dl><dd>--c++-kinds=+p</dd><dd>--fields=+iaS</dd><dd>--extra=+q</dd><dd>--language-force=C++</dd></dl>From a console (the exclude options may vary) generate the tags file as follows:
<pre>ctags --exclude=.svn --exclude=target -R .</pre>