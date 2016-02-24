---
layout: post
title: "Xcode常用插件大法"
date: 2015-10-30 10:03:35 +0800
comments: true
thumbnail: /img/2015/10/10301003.jpg
cover: /img/2015/10301003b.jpg
bootstrap: true
categories: 
---

正所谓--工欲善其事，必先利其器<!--more-->

　　目前版主使用的是Xcode7.1，基本上以下插件都安装使用过，实用性还是非常强的，这些插件可以用插件管理器**alcatraz**来进行管理；当使用插件的时候部分没法使用的插件可以会导致整个插件库都无法使用，所以有些插件不可使用的话最好不要安装，如第一个codeSnippet插件，在版主安装之后导致全部插件无法使用，包括**alcatraz**，具体可以检查**alatraz**是否存在于menu->Window->Packge Manager这个包管理选项，如存在说明未失效，插件可用；如果消失，说明有插件无法使用导致整个插件库不可使用，需要一一排除。

###ACCodeSnippetRepositoryPlugin
ACCodeSnippetRepositoryPlugin is a Xcode plugin for seemless synchronization of snippets with a git repository.

The snippets are synchronized as human-readable text (and not an obscure .codesnippet plist).

Build Status

If you want to know more about snippets in Xcode, I recommend reading [Xcode Snippets on NSHipster](http://nshipster.com/xcode-snippets/).

Want to try the plugin with an existing public repository? Try [acoomans](https://github.com/acoomans/xcode-snippets) or [mattt's](https://github.com/mattt/Xcode-Snippets.git) snippets.

注：目前作者并未做对于Xcode6.2的支持

###AdjustFontSize
A simple plugin for Xcode to adjust font size without going into Settings → Fonts & Colors and changing each source type.

Simply hit ⌘ + or ⌘ - and all fonts will be adjusted. Plugin respects different font sizes per each syntax type.

NOTE keep in mind that it modifies the current theme file.

###AllTargets
AllTargets is a plugin for Xcode. The plugin intends to auto select all targets when you add files to the project.


###Auto Importer for Xcode(Xcode6.2不可使用)
Quickly import your headers on the fly without having to manually go to the top of your file and type the import statement.


####Usage

⌘ + ctrl + H after selecting some text (or you can have no selection at all)
If the selected text matches the name of a class/protocol or category method, it will import the header and you're done, otherwise it will show a list of filtered identifiers and headers...
start typing the keyword of your import
use ↑ or ↓ keys to navigate
press ↵ or double click to add an import
NOTE: on the list, classes are shown as [C], protocols as [P] and category methods as [ClassExtended()]

###BBUDebuggerTuckAway
Xcode plugin for auto-hiding the debugger once you start typing in the source code editor.

###BBUUtilitiesTuckAway
Xcode plugin for auto-hiding the utilities area once you start typing in the source code editor.

###BlockJump  
在方法之间跳转  
Default Key Combination:

CTRL + [ : jump up  
CTRL + ] : jump down

CocoaControlsPlugin  
OS X native application with Xcode plugin for browsing, searching, integrating, cloning controls in [http://cocoacontrols.com/](http://cocoacontrols.com/).  

All the commands are laid at the bottom of the menu View.

Use the menu Cocoa Controls to open CocoaControls.app immediately.  
Click on the left image view to open the image in a new window.  
Click on the pod button to integrate pod.  
Click on the computer button to clone the source.  
Double click on the cell view to open the control in browser.


DXXcodeConsoleUnicodePlugin  
转换 Xcode 控制台中一些不可阅读的字符。  
######使用方法：
两种：

1.快捷键 option+c 会转换当前 剪贴板 中的内容并用一个对话框把转换后的内容显示出来。

2.在 Xcode 的 Edit 菜单中勾选 ConvertUnicodeInConsole(Beta)，然后 console 中再出现 \u4e0e 时，就会自动转换成 与 显示。

IntelliPaste-for-XCode  
在.m文件中选中方法函数后cmd+c，在头文件使用shift+cmd+v将只复制出方法名；  


###CATweaker
A helper tool for creating beautiful CAMediaTimingFunction curve.

### LinkedLog Xcode Plugin
LinkedLog is a Xcode plugin that includes a Xcode PCH file template that adds the macros LLog and LLogF. The LLog macro will work like NSLog but additionally prints the file name and line number of the call.

LinkedLog then parses the logs and adds links to the corresponding file and line.
可直接在console上直接定位log位置，并可以直接跳转到该行代码文件；  

### OMColorSense
ColorSense is an Xcode plugin that makes working with UIColor (and NSColor) more visual.
在输入颜色代码时显示具体颜色;

### VVDocumenter-Xcode
大神的需要，///就可以给出注释格式

### XBookmark
书签管理器，定位器除了断点外的好使用

### Xcode Delete Line
command+D 删除行，增加代码键盘手的操作性

### HTYCopyIssue
Makes Copy Xcode Issue Description Easy, Support Finding Answers in Google or StackOverflow Directly；  
如果有些错误和waring想复制去谷歌的时候会连位置和问题都复制，操作麻烦，这个插件就可以直接复制出问题来；

### FuzzyAutocomplete
补全代码功能，例:dictionaryWithContentsOfFile:方法直接输入dictWFile即可找到

### DXXcodeConsoleUnicodePlugin
将终端输出的Unicode编码转换成中文，这个很实用
统一删除插件方法  
	
		rm -r ~/Library/Application\ Support/Developer/Shared/Xcode/Plug-ins/AutoImporter.xcplugin/

以上就是我用的比较常的插件，记住，插件有时候还会导致Xcode出现卡顿或者闪退的现象，所以插件这东西有好有坏，大家慎重考虑，不强求；一下是本人使用的插件图：  
![](/img/2015/10/XcodePlugin.png)