# 任务(Task)

vscode中的任务功能是用来集成外部工具的，事先配置好外部工具的命令及参数，
通过任务功能就可以快速调用这些命令，并捕获其输出显示在集成终端中。

根据任务目的的不同，vscode将任务分为3组：

+ build
+ test
+ none

编译组的任务有一些特别优待：
+ 可以配置Problem Matcher，提取编译结果中的错误、警告提示，点击跳转到对应代码行。
+ 专属快捷键`Ctrl+Shift+B`，其它组任务需要通过菜单或命令面板触发。

# 基本配置

vscode内置了一些常见任务的配置模板，首次运行任务时，会让用户选择任务类型，
然后生成默认的配置文件`.vscode/task.json`。

下面是一个典型的最小配置文件。

```json
{
    // See https://go.microsoft.com/fwlink/?LinkId=733558
    // for the documentation about the tasks.json format
    "version": "2.0.0",
    "tasks": [
        {
            "label": "run-py",
            "type": "shell",
            "command": "py",
            "args":[
                "${file}"
            ],
        }
    ]
}
```

`task`项是一个数组，显而易见可以配置多个任务。

各项配置说明如下：

+ **label**: 任务的名字，用以区分不同的任务。
+ **type**: 任务类型，区别主要在于参数的配置方式上，详见**command**。
    - `shell`: 表明命令需要在shell中执行。
    - `process`: 命令本身作为一个独立的进程执行。
+ **command**: 要执行的外部命令。对于`shell`类型的命令，必须同时指定参数，如`test.sh arg1`。
+ **args**: 命令执行时的参数。`process`类型的命令才可以单独配置参数。**TODO: 根据实测，似乎`shell`也能单独配置参数。**
+ **windows**: Windows平台特定配置，如果在Windows上运行，其下配置会覆盖相应的上级配置。
+ **group**: 任务的分组信息，有两种写法：
    ```json
    "group": "build"        // 为build/test/none其中之一
    ```
    或
    ```json
    "group": {
        "kind": "build",    // 只能为build或test
        "isDefault": true   // 只能为true，有多个任务时，默认执行这一个
    },
    ```
    `build`分组的任务，可通过快捷键`Ctrl+Shift+B`运行，
+ **presentation**: 界面上任务终端窗格的显示方式。
    - **reveal**: 终端窗格显示方式。
        + `always`: 总是显示
        + `never`: 手动显示
        + `silent`: **TODO:意义是什么？**
    - **panel**: 终端的共享方式
        + `shared`: 每次运行的输出都追加到同一个终端
        + `dedicated`: 一种任务创建一个终端
        + `new`: 每次运行都创建新的终端。
    - **focus**: 终端是否获得焦点，默认为`false`。
    - **echo**: 终端是否显示实际执行的命令，默认为`true`。

# 内置变量

vscode内置了一些变量，用以代表当前文件、工作区目录等内容，供配置文件使用。

假设当前打开的工作区目录是`d:\workshop`，当前打开的文件是`sub/draft.txt`，光标位于第`7`行，
则各内置变量含义及取值如下：

|           变量             |           值              |   含义  |
|----------------------------|--------------------------|---------------------------
| ${workspaceFolder}         | 工作区目录                | d:\workshop               
| ${workspaceFolderBasename} | 工作区目录名              | workshop                  
| ${file}                    | 当前文件                  | d:\workshop\sub\draft.txt 
| ${relativeFile}            | 当前文件相对于工作区的路径  | sub/draft.txt             |
| ${fileBasename}            | 当前文件名                | draft.txt                 
| ${fileBasenameNoExtension} | 当前文件名，不含扩展名     | draft                     
| ${fileDirname}             | 当前文件所在目录          |  d:\workshop\sub           
| ${fileExtname}             | 当前文件扩展名            | .txt                      
| ${cwd}                     | 任务工作目录              |  d:\workshop               
| ${lineNumber}              | 当前文件的光标所在行       |  7                         
| ${env:Name}                | 系统环境变量              |  {Name}环境变量            
||

# 操作系统特定配置

task.json中还可以针对特定操作系统单独配置，在相应操作系统上运行时，特定设置会覆盖通用设置。

```json
{
    "label": "Run Node",
    "type": "process",
    "windows": {
        "command": "C:\\Program Files\\nodejs\\node.exe"
    },
    "linux": {
        "command": "/usr/bin/node"
    }
}
```

支持的操作系统如下：

+ **windows**: Windows
+ **linux**: Linux
+ **osx**: macOS

# Problem Matcher

对于代码编译型的任务，可以将编译的输出按照固定的格式解析，提取出编译错误及警告等信息，
显示在终端窗格中，双击即可跳转到问题代码处，类似IDE的功能，便于排错。

**TODO: PLACEHOLDER**
