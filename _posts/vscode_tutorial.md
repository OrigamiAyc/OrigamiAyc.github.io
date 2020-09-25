---
layout:		post
title:		VS code，从编辑器到编译器
date:		03/08/2020 (First-time Published); 09/19/2020 (Copied Here)
author:		上官凝
catalog:	true
# tags:
	# - Tutorials & Tricks
	# - Development Tools
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
            ], // 这个路径改成自己的（但好像不是很重要？我现在没有Xcode了但是还能正常编译
            "compilerPath": "/usr/bin/clang", // 这个很重要，替换成自己的
            "cStandard": "c11",
            "cppStandard": "c++17",
            "intelliSenseMode": "clang-x64"
        }
    ],
    "version": 4
}
```
如果你不知道你的 `clang` 在哪里，可以
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
        "name": "(gdb) Launch", // 配置名称，将会在启动配置的下拉菜单中显示
        "type": "cppdbg", // 配置类型，cppdbg对应cpptools提供的调试功能；可以认为此处只能是cppdbg
        "request": "launch", // 请求配置类型，可以为launch（启动）或attach（附加）
        "program": "${fileDirname}/${fileBasenameNoExtension}.exe", // 将要进行调试的程序的路径
        "args": [], // 程序调试时传递给程序的命令行参数，一般设为空即可
        "stopAtEntry": false, // 设为true时程序将暂停在程序入口处，相当于在main上打断点
        "cwd": "${workspaceFolder}", // 调试程序时的工作目录，此为工作区文件夹；改成${fileDirname}可变为文件所在目录
        "environment": [], // 环境变量
        "externalConsole": true, // 为true时使用单独的cmd窗口，与其它IDE一致；18年10月后设为false可调用VSC内置终端
        "internalConsoleOptions": "neverOpen", // 如果不设为neverOpen，调试时会跳到“调试控制台”选项卡，你应该不需要对gdb手动输命令吧？
        "MIMode": "gdb", // 指定连接的调试器，可以为gdb或lldb。但我没试过lldb
        "miDebuggerPath": "gdb.exe", // 调试器路径，Windows下后缀不能省略，Linux下则不要
        "setupCommands": [
            { // 模板自带，好像可以更好地显示STL容器的内容，具体作用自行Google
                "description": "Enable pretty-printing for gdb",
                "text": "-enable-pretty-printing",
                "ignoreFailures": false
            }
        ],
        "preLaunchTask": "Compile" // 调试会话开始前执行的任务，一般为编译程序。与tasks.json的label相对应
    }]
}
```
###### Mac 版
```json
{
	// Use IntelliSense to learn about possible attributes.
	// Hover to view descriptions of existing attributes.
	// For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
	"version": "0.2.0",
	"configurations": [
		{
			"name": "(lldb) 启动",
			"type": "cppdbg",
			"request": "launch",
			"program": "${fileDirname}/${fileBasenameNoExtension}",
			"args": [],
			"stopAtEntry": false, // 设为true时程序将暂停在程序入口处，相当于在main上打断点
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
			// "preLaunchTask": "Compile with g++",
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
        "label": "Compile", // 任务名称，与launch.json的preLaunchTask相对应
        "command": "gcc",   // 要使用的编译器，C++用g++
        "args": [
            "${file}",
            "-o",    // 指定输出文件名，不加该参数则默认输出a.exe，Linux下默认a.out
            "${fileDirname}/${fileBasenameNoExtension}.exe",
            "-g",    // 生成和调试有关的信息
            "-Wall", // 开启额外警告
            "-static-libgcc",     // 静态链接libgcc，一般都会加上
            "-fexec-charset=GBK", // 生成的程序使用GBK编码，不加这一条会导致Win下输出中文乱码
            // "-std=c11", // C++最新标准为c++17，或根据自己的需要进行修改
        ], // 编译的命令，其实相当于VSC帮你在终端中输了这些东西
        "type": "process", // process是vsc把预定义变量和转义解析后直接全部传给command；shell相当于先打开shell再输入命令，所以args还会经过shell再解析一遍
        "group": {
            "kind": "build",
            "isDefault": true // 不为true时ctrl shift B就要手动选择了
        },
        "presentation": {
            "echo": true,
            "reveal": "always", // 执行任务时是否跳转到终端面板，可以为always，silent，never。具体参见VSC的文档
            "focus": false,     // 设为true后可以使执行task时焦点聚集在终端，但对编译C/C++来说，设为true没有意义
            "panel": "shared"   // 不同的文件的编译信息共享一个终端面板
        },
        // "problemMatcher":"$gcc" // 此选项可以捕捉编译时终端里的报错信息；但因为有Lint，再开这个可能有双重报错
    }]
}
```
###### Mac
```json
{
	// See https://go.microsoft.com/fwlink/?LinkId=733558
	// for the documentation about the tasks.json format
	"version": "2.0.0",
	"tasks": [
		{
			"label": "build",
			"type": "shell",
			"command": "msbuild",
			"args": [
				// Ask msbuild to generate full paths for file names.
				"/property:GenerateFullPaths=true",
				"/t:build",
				// Do not generate summary otherwise it leads to duplicate errors in Problems panel
				"/consoleloggerparameters:NoSummary",
				// "${file}",
				// "-g", // 生成和调试有关的信息
				// "-Wall", // 开启额外警告
				"-o", // 指定输出文件名，不加该参数则默认输出a.exe，Linux下默认a.out
				"${fileDirname}/${fileBasenameNoExtension}.out",
				"-static-libgcc" // 静态链接libgcc，一般都会加上
			],
			"group": {
				"kind": "build",
				"isDefault": true
			},
			"presentation": {
				// Reveal the output only if unrecognized errors occur.
				"reveal": "silent"
			},
			// Use the standard MS compiler pattern to detect errors, warnings and infos
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


    "files.defaultLanguage": "cpp", // ctrl+N新建文件后默认的语言
    "files.trimTrailingWhitespace": true, // 保存时，删除每一行末尾的空格
    "files.insertFinalNewline": true,// 保存后文件最末尾加一整行空行，Linux下的习惯

    "C_Cpp.updateChannel": "Insiders",
    "http.proxySupport": "off",
    "vsicons.dontShowNewVersionMessage": true,
    "window.zoomLevel": -1,
    "workbench.iconTheme": "vscode-icons",
    "vsintellicode.modify.editor.suggestSelection": "automaticallyOverrodeDefaultValue",
    "workbench.colorTheme": "One Dark Pro",

    // editor
    "editor.fontSize": 21,
    "editor.largeFileOptimizations": false,
    "editor.dragAndDrop": true,
    "editor.formatOnType": true, // 输入时就进行格式化，默认触发字符较少，分号可以触发
    "editor.snippetSuggestions": "top", // snippets代码优先显示补全
    "editor.suggestSelection": "first",
    "editor.insertSpaces": false,
    "editor.cursorStyle": "line-thin",
    "editor.formatOnPaste": true,
    "editor.multiCursorModifier": "ctrlCmd",
    "editor.fontLigatures": true, // 连体字，效果不太好形容，见 https://typeof.net/Iosevka 最后一部分
    "editor.cursorSmoothCaretAnimation": true, // 移动光标时变得平滑
    "editor.smoothScrolling": true, // 滚动平滑，不过效果很微弱
    // "editor.fontFamily": "Menlo, Monaco, 'Courier New', monospace",
    "editor.fontFamily": "Consolas, 'Courier New', monospace",
    "editor.fontWeight": "normal",

    // code-runner
    "code-runner.runInTerminal": true, // 设置成false会在“输出”中输出，无法输入
    "code-runner.preserveFocus": true, // 若为false，run code后光标会聚焦到终端上。如果需要频繁输入数据可设为false
    "code-runner.clearPreviousOutput": false,
    "terminal.integrated.cursorBlinking": true,
    "terminal.integrated.cursorStyle": "line",
    "terminal.integrated.fontSize": 20,
    // "code-runner.executorMap": {
    //     // "c": "cd $dir && gcc '$fileName' -o '$fileNameWithoutExt.out' -Wall -g -O2 -static-libgcc -std=c11 -fexec-charset=GBK && &'$dir$fileNameWithoutExt'",
    //     // "cpp": "cd $dir && g++ '$fileName' -o '$fileNameWithoutExt.out' -Wall -g -O2 -static-libgcc -std=c++17 -fexec-charset=GBK && &'$dir$fileNameWithoutExt'"
    //     "c": "cd $dir; clang $fileName -o $fileNameWithoutExt.exe -Wall -g -Og -static-libgcc -fcolor-diagnostics --target=x86_64-w64-mingw -std=c11; ./$fileNameWithoutExt",
    //     "cpp": "cd $dir; clang++ $fileName -o $fileNameWithoutExt.exe -Wall -g -Og -static-libgcc -fcolor-diagnostics --target=x86_64-w64-mingw -std=c++17; ./$fileNameWithoutExt"
    // }, // 设置code runner的命令行

    // python
    "python.pythonPath": "/usr/local/bin/python3",
    "diffEditor.ignoreTrimWhitespace": true,
    "debug.allowBreakpointsEverywhere": true,

    // latex
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
