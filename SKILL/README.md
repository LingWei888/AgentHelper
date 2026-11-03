# SKILL Scripts Repository

欢迎来到 SKILL 专属开发文件夹！本目录专门用于存放、管理和维护所有与 SKILL 相关的脚本、函数库及配置文件。

## 1. SKILL 的本质含义

**SKILL** 是 Cadence Design Systems 开发的一种高级交互式编程语言，主要用于其电子设计自动化（EDA）软件（如 Virtuoso 平台）。
* **本质属性**：它是一种基于 LISP（LISt Processing）方言的脚本语言，支持面向对象编程（Skill++）。
* **核心作用**：通过调用 Cadence 提供的海量底层 API，实现版图设计（Layout）、原理图绘制（Schematic）、参数化单元（PCell）开发以及工作流程的自动化。
* **优势**：极大地提高 IC 工程师和版图工程师的工作效率，替代大量重复的手工点击操作。

## 2. 目录结构说明

为了保持代码的可维护性和整洁性，本文件夹采用以下标准目录结构：

```text
📦 SKILL-Workspace
 ┣ 📂 src/              # 存放核心业务逻辑和主要的 SKILL 脚本源码 (.il)
 ┃ ┣ 📜 auto_route.il   # (示例) 自动连线脚本
 ┃ ┗ 📜 check_drc.il    # (示例) 辅助检查脚本
 ┣ 📂 lib/              # 通用函数库、依赖文件和封装好的公共模块
 ┃ ┗ 📜 utils.il        # (示例) 通用数学或字符串处理工具
 ┣ 📂 pcells/           # 专门存放参数化单元 (PCell) 的开发脚本
 ┣ 📂 ui/               # 存放图形用户界面 (GUI) 相关的代码 (如表单、菜单)
 ┣ 📂 tests/            # 测试脚本，用于在隔离环境中验证代码功能
 ┣ 📂 docs/             # 存放设计文档、API 速查表或学习笔记
 ┣ 📜 .cdsinit          # (可选) Cadence 环境初始化文件，用于自动加载必要脚本
 ┗ 📜 README.md         # 本说明文档
```

## 3. 基本用法 (Usage)

在 Cadence Virtuoso 环境中加载和使用本文件夹中的 SKILL 脚本，通常有以下几种方式：

### 3.1 交互式窗口 (CIW) 手动加载
在 Virtuoso 的 Command Interpreter Window (CIW) 命令行中，输入以下命令来加载并编译您的代码：
```skill
; 加载单个脚本 (如果在当前目录下)
load("src/your_script.il")

; 加载并显示详细信息
loadi("src/your_script.il")
```

### 3.2 自动加载配置
如果您希望每次打开 Virtuoso 时自动加载某些核心函数，可以将加载路径添加到 `.cdsinit` 文件中：
```skill
let( (skill_path)
    skill_path = "~/your_path/SKILL-Workspace/src/your_script.il"
    when( isFile(skill_path)
        load(skill_path)
    )
)
```

### 3.3 执行函数
脚本加载成功后，直接在 CIW 中调用您在脚本里定义的函数名即可：
```skill
; 假设您的脚本中定义了 procedure( MyAutoRoute() ... )
MyAutoRoute()
```

## 4. 开发规范与注意事项

为了写出高质量的 SKILL 代码，建议遵循以下原则：
1. **命名空间前缀**：为您的自定义函数和全局变量添加独特的前缀（如您的名字缩写 `ABC_`），避免与 Cadence 内部函数冲突。例如：`procedure( ABC_calculateArea() ... )`。
2. **局部变量声明**：始终使用 `let` 或 `prog` 来声明局部变量，严格防止全局变量污染。
3. **注释习惯**：使用 `;` 进行单行注释，或使用 `/* ... */` 进行块注释。为复杂的 procedure 提供清晰的输入输出说明。
4. **文件后缀**：标准 SKILL 代码一般以 `.il` 为后缀，Skill++（面向对象）通常以 `.ils` 为后缀。

## 5. 参考资料
* Cadence SKILL Language Reference (官方帮助文档，可在终端输入 `cdnshelp` 查阅)
* SKILL IDE (Virtuoso 内置的集成开发环境)

---
*文档更新时间：2026年*