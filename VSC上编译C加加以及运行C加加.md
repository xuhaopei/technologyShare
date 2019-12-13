# **VSC上编译C++以及运行C++**

## 下载MinGw编译器

我安装的是dev-c++编辑器，里面已经有MinGW编译器了。

## 配置launch.json文件

{

  // 使用 IntelliSense 了解相关属性。 

  // 悬停以查看现有属性的描述。

  // 欲了解更多信息，请访问: https://go.microsoft.com/fwlink/?linkid=830387

  "version": "0.2.0", 

  "configurations": [ 

​    

​    { 

​      "name": "(gdb) Launch", // 配置名称，将会在启动配置的下拉菜单中显示 

​      "type": "cppdbg",    // 配置类型，这里只能为cppdbg 

​      "request": "launch",  // 请求配置类型，可以为launch（启动）或attach（附加） 

​      "program": "${workspaceRoot}/${fileBasenameNoExtension}.exe",// 将要进行调试的程序的路径 

​      "args": [],       // 程序调试时传递给程序的命令行参数，一般设为空即可 

​      "stopAtEntry": false,  // 设为true时程序将暂停在程序入口处，一般设置为false 

​      "cwd": "${workspaceRoot}", // 调试程序时的工作目录，一般为${workspaceRoot}即代码所在目录 

​      "environment": [], 

​      "externalConsole": true, // 调试时是否显示控制台窗口，一般设置为true显示控制台 

​      "MIMode": "gdb", 

​      "miDebuggerPath": "D:\\Dev-C\\Dev-Cpp\\MinGW64\\bin\\gdb.exe", // miDebugger的路径，注意这里要与MinGw的路径对应 

​      "preLaunchTask": "D:\\Dev-C\\Dev-Cpp\\MinGW64\\bin\\g++.exe", // 调试会话开始前执行的任务，一般为编译程序，c++为g++, c为gcc 

​      "setupCommands": [ 

​        {  

​      "description": "Enable pretty-printing for gdb", 

​          "text": "-enable-pretty-printing", 

​          "ignoreFailures": true 

​        } 

​      ] 

​    } 

  ] 

}

 

## 配置taks.json文件

{

  // See https://go.microsoft.com/fwlink/?LinkId=733558

  // for the documentation about the tasks.json format

  "version": "2.0.0",

  "command": "D:\\Dev-C\\Dev-Cpp\\MinGW64\\bin\\g++.exe",

  "args": ["-g","${file}","-o","${fileBasenameNoExtension}.exe"],  // 编译命令参数

  "problemMatcher": {

​    "owner": "cpp",

​    "fileLocation": ["relative", "${workspaceRoot}"],

​    "pattern": {

​      "regexp": "^(.*):(\\d+):(\\d+):\\s+(warning|error):\\s+(.*)$",

​      "file": 1,

​      "line": 2,

​      "column": 3,

​      "severity": 4,

​      "message": 5

​    }

  }

}

## 配置c_cpp_properties.json文件

{

  "configurations": [

​    {

​      "name": "Win32",

​      "includePath": [

​        "${workspaceFolder}/**",

​        "D:\\Dev-C\\Dev-Cpp\\MinGW64\\x86_64-w64-mingw32\\include",   //引入库文件

​        "D:\\test.h"                           //头文件

​      ],

​      "defines": [

​        "_DEBUG",

​        "UNICODE",

​        "_UNICODE"

​      ],

​      "intelliSenseMode": "msvc-x64"

​    }

  ],

  "version": 4

}

 