---
layout:		post
title:		VS code，从编辑器到编译器
subtitle:   Visual Studio Code 对C/C++的初步配置
date:		03/08/2020
author:		上官凝
catalog:	true
tags:
<<<<<<< HEAD
	- Tutorials & Tricks
	- Development Tools
=======
    - Tutorials & Tricks
    - Development Tools
>>>>>>> a2bb1ac49f00c8413facb5c3b8bffcdef49a285d
---

很多人都把 vs code(vsc) 作为一个好康的编辑器来用（yysy确实比vim、emacs好看多了）不过一个 开源的、跨平台的 的vs code，更应该发挥出“不是IDE优于IDE”的力量。

下面以 cpp 开发环境为例，介绍一下 vsc 的配置过程。
## 打开 vsc 之前
首先是环境变量，win 下是用 `MinGW-w64 - for 32 and 64 bit Windows`，选新版本的 `x86_64-posix-seh`。

macOS 相对简单，Xcode 大法完事···或者采用Xcode-select也可以
## 选择插件
vsc 的 extension 商城琳琅满目的插件，不过其实就两个足矣。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200308211321416.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTQ5NDgxMQ==,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200308211347214.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTQ5NDgxMQ==,size_16,color_FFFFFF,t_70)
有了这两个插件，就可以准备编译运行了！
## json 文件
有 3 个 `json` 文件是与编译运行相关的，（基本上复制粘贴就行）分别是：
#### `c_cpp_properties.json`
```json
{
    "configurations": [
        {
            "name": "Mac",
            "includePath": [
                "${workspaceFolder}/**"
            ],
            "defines": [],
            "macFrameworkPath": [
                "/Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX.sdk/System/Library/Frameworks"
            ],
            "compilerPath": "/usr/bin/clang",
            "cStandard": "c11",
            "cppStandard": "c++17",
            "intelliSenseMode": "clang-x64"
        }
    ],
    "version": 4
}
```
clang 要换成自己电脑上的绝对路径，如果你不知道你的 `clang` 在哪里，可以
```shell
$ whereis clang
```
win 是你自己下载的 mingw 的路径。。。
#### `launch.json`
###### win 版
```json
{
    "version": "0.2.0",
    "configurations": [{
        "name": "(gdb) Launch",
        "type": "cppdbg",
        "request": "launch",
        "program": "${fileDirname}/${fileBasenameNoExtension}.exe",
        "args": [],
        "stopAtEntry": false,
        "cwd": "${workspaceFolder}",
        "environment": [],
        "externalConsole": true,
        "internalConsoleOptions": "neverOpen",
        "MIMode": "gdb",
        "miDebuggerPath": "gdb.exe",
        "setupCommands": [
            {
                "description": "Enable pretty-printing for gdb",
                "text": "-enable-pretty-printing",
                "ignoreFailures": false
            }
        ],
        "preLaunchTask": "Compile"
    }]
}
```
###### Mac 版
```json
{
	"version": "0.2.0",
	"configurations": [
		{
			"name": "(lldb) 启动",
			"type": "cppdbg",
			"request": "launch",
			"program": "${fileDirname}/${fileBasenameNoExtension}",
			"args": [],
			"stopAtEntry": false,
			"cwd": "${workspaceFolder}",
			"environment": [],
			"externalConsole": false,
			"MIMode": "lldb",
			"miDebuggerPath": "/usr/bin/clang",
			"setupCommands": [
				{
					"description": "Enable pretty-printing for gdb",
					"text": "-enable-pretty-printing",
					"ignoreFailures": true
				}
			],
		}
	]
}
```
#### `tasks.json`
###### win
```json
{
    "version": "2.0.0",
    "tasks": [{
        "label": "Compile",
        "command": "gcc",
        "args": [
            "${file}",
            "-o",
            "${fileDirname}/${fileBasenameNoExtension}.exe",
            "-g",
            "-Wall",
            "-static-libgcc",
            "-fexec-charset=GBK",
        ],
        "type": "process",
        "group": {
            "kind": "build",
            "isDefault": true
        },
        "presentation": {
            "echo": true,
            "reveal": "always",
            "focus": false,
            "panel": "shared"
        },
    }]
}
```
###### Mac
```json
{
	"version": "2.0.0",
	"tasks": [
		{
			"label": "build",
			"type": "shell",
			"command": "msbuild",
			"args": [
				"/property:GenerateFullPaths=true",
				"/t:build",
				"/consoleloggerparameters:NoSummary",
				"-o",
				"${fileDirname}/${fileBasenameNoExtension}.out",
				"-static-libgcc"
			],
			"group": {
				"kind": "build",
				"isDefault": true
			},
			"presentation": {
				"reveal": "silent"
			},
			"problemMatcher": "$msCompile"
		}
	]
}
```
## 一些辅助性的设置
#### 好康的扩展
这个会让你有明亮的括号
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200308212719538.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTQ5NDgxMQ==,size_16,color_FFFFFF,t_70)
这个是一个比较省眼睛的主题
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200308212820789.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTQ5NDgxMQ==,size_16,color_FFFFFF,t_70)
还有就是：
#### `settings.json`
1. 在左下角找设置
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200308212901851.png)
2. 右上角打开 json（就是三角形右边那个按钮）
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200308213035400.png)
3. 然后选择你需要的部分输入进去
```json
{


    "files.defaultLanguage": "cpp",
    "files.trimTrailingWhitespace": true,
    "files.insertFinalNewline": true,

    "C_Cpp.updateChannel": "Insiders",
    "http.proxySupport": "off",
    "vsicons.dontShowNewVersionMessage": true,
    "window.zoomLevel": -1,
    "workbench.iconTheme": "vscode-icons",
    "vsintellicode.modify.editor.suggestSelection": "automaticallyOverrodeDefaultValue",
    "workbench.colorTheme": "One Dark Pro",

    "editor.fontSize": 21,
    "editor.largeFileOptimizations": false,
    "editor.dragAndDrop": true,
    "editor.formatOnType": true,
    "editor.snippetSuggestions": "top",
    "editor.suggestSelection": "first",
    "editor.insertSpaces": false,
    "editor.cursorStyle": "line-thin",
    "editor.formatOnPaste": true,
    "editor.multiCursorModifier": "ctrlCmd",
    "editor.fontLigatures": true,
    "editor.cursorSmoothCaretAnimation": true,
    "editor.smoothScrolling": true,
    "editor.fontFamily": "Consolas, 'Courier New', monospace",
    "editor.fontWeight": "normal",

    "code-runner.runInTerminal": true,
    "code-runner.preserveFocus": true,
    "code-runner.clearPreviousOutput": false,
    "terminal.integrated.cursorBlinking": true,
    "terminal.integrated.cursorStyle": "line",
    "terminal.integrated.fontSize": 20,

    "python.pythonPath": "/usr/local/bin/python3",
    "diffEditor.ignoreTrimWhitespace": true,
    "debug.allowBreakpointsEverywhere": true,

    "latex-workshop.latex.recipes": [
        {
            "name": "xelatex",
            "tools": [
                "xelatex"
            ]
        },
        {
            "name": "latexmk",
            "tools": [
                "latexmk"
            ]
        },
        {
            "name": "pdflatex -> bibtex -> pdflatex*2",
            "tools": [
                "pdflatex",
                "bibtex",
                "pdflatex",
                "pdflatex"
            ]
        }
    ],
    "latex-workshop.latex.tools": [
        {
            "name": "latexmk",
            "command": "latexmk",
            "args": [
                "-synctex=1",
                "-interaction=nonstopmode",
                "-file-line-error",
                "-pdf",
                "%DOC%"
            ]
        },
        {
            "name": "xelatex",
            "command": "xelatex",
            "args": [
                "-synctex=1",
                "-interaction=nonstopmode",
                "-file-line-error",
                "%DOC%"
            ]
        },
        {
            "name": "pdflatex",
            "command": "pdflatex",
            "args": [
                "-synctex=1",
                "-interaction=nonstopmode",
                "-file-line-error",
                "%DOC%"
            ]
        },
        {
            "name": "bibtex",
            "command": "bibtex",
            "args": [
                "%DOCFILE%"
            ]
        }
    ],
    "latex-workshop.view.pdf.viewer": "tab",
    "latex-workshop.latex.clean.fileTypes": [
        "*.aux",
        "*.bbl",
        "*.blg",
        "*.idx",
        "*.ind",
        "*.lof",
        "*.lot",
        "*.out",
        "*.toc",
        "*.acn",
        "*.acr",
        "*.alg",
        "*.glg",
        "*.glo",
        "*.gls",
        "*.ist",
        "*.fls",
        "*.log",
        "*.fdb_latexmk"
    ],
}
```
> 好了到此结束，享受你的 visual studio code 吧！

哦对了，如果想更深了解，可以参考[这条](https://www.zhihu.com/question/30315894)
