# Note —— 个人桌面事务管理系统

**Note 是一款基于 Python 开发的 Windows 本地个人事务管理工具，集待办事项、桌面提醒、便签、日记、收支记账和日历统计于一体。**

项目使用 Python、Tkinter 和 SQLite 构建，桌面组件用于随手记录和处理事项，管理窗口提供分类、编辑与按日期回看。系统无需注册账号或配置 API Key，数据保存在本地数据库中。

## 一、核心功能

### 1. 待办事项管理
- 支持待办事项的创建、编辑、删除，以及完成和恢复。
- 支持任务分类、颜色管理、截止时间和定时提醒。
- 桌面组件默认显示前三项任务，可展开查看并拖动排序。

### 2. 桌面提醒
- 在桌面组件中显示事项名称和分类。
- 支持定时检查及提醒展示，点击提醒可将其收起。
- 提供“30 秒后提醒”的演示设置。

### 3. 个性化便签
- 提供黄色、蓝色、粉色、白底、照片和并排等便签样式。
- 支持文字编辑、自动保存和图片上传。
- 可选择便签内容在桌面展示。

### 4. 日记管理
- **情绪日记：** 按事实、感受、需求三个维度记录内容，支持心情和 Emoji 多选。
- **四象限日记：** 提供复盘和“一日四分”两种记录形式，也支持自定义栏目标题。
- 支持按日期保存和查看历史日记。

### 5. 收支记账
- 支持收入、支出、分类和备注记录。
- 支持已有记录的修改和删除。
- 使用整数分进行金额计算，并提供每日收入、支出和结余统计。

### 6. 日历统计
- 按周一至周日排列展示月历。
- 每日显示收支以及便签、日记数量。
- 支持查看所选日期的详细记录。

## 二、技术栈

| 技术 | 用途 |
|---|---|
| Python 3.12 | 核心开发语言 |
| Tkinter | 桌面图形用户界面 |
| SQLite | 本地数据存储 |
| Pillow 13.0.0 | 图片读取与处理 |
| unittest | 自动化测试 |

项目主要采用模块化设计，通过统一的数据访问模块连接业务功能与 SQLite 数据库。典型数据流程为：

**界面回调 → 输入校验 → Store 执行 SQL → SQLite 提交 → 主窗口和桌面组件刷新。**

## 三、运行环境

- Windows 10 / Windows 11，64 位
- Python 3.12，安装时需包含 Tcl/Tk
- Pillow 13.0.0

桌面组件使用 Windows 专用窗口属性，当前暂未适配 macOS、Linux 或无桌面的服务器环境。

## 四、安装与运行

### 方式一：使用项目脚本

1. 安装 Python 3.12，并确保包含 `pip`、`venv` 和 Tcl/Tk。
2. 下载并解压本项目。
3. 在项目根目录双击 `setup.cmd`，创建虚拟环境并安装依赖。
4. 双击 `run.cmd` 启动程序。

### 方式二：使用 PowerShell

在项目根目录打开 PowerShell，依次执行：

```powershell
py -3.12 -m venv .venv
.\.venv\Scripts\python.exe -m pip install -r requirements.txt
.\.venv\Scripts\python.exe app.py
```

运行时唯一的第三方依赖是 Pillow；Tkinter、sqlite3、unittest 等由 Python 提供。

## 五、第一次使用

1. 点击“添加事项”，输入事项名称、分类和可选时间，保存后可在桌面组件和管理列表中查看。
2. 设置提醒时间并保存待办事项。程序运行期间会定时检查提醒。
3. 拖动桌面任务标题可调整顺序；点击完成按钮可更新任务状态。
4. 使用桌面组件中的“+”入口添加便签、日记或账单。
5. 在日历中选择日期，可查看当天的便签、日记和记账详情。

关闭管理窗口通常只是隐藏窗口，桌面组件和提醒仍会运行。如需完全退出，请在桌面组件的“···”菜单中选择“退出 Note”。

## 六、数据保存与备份

默认数据库位置：

```text
%LOCALAPPDATA%\CourseDesk\course_desk.sqlite3
```

数据库与源码、可执行文件分开保存，便签图片也存放在数据库中。项目没有云同步功能，数据库未加密，请妥善保管。

备份时请先完全退出 Note，再复制数据库文件到安全位置；恢复数据库前也应先退出程序。

如需隔离演示数据，可在当前 PowerShell 会话中指定另一份数据库：

```powershell
$env:COURSE_DESK_DB = Join-Path $PWD 'local-data\demo.sqlite3'
.\.venv\Scripts\python.exe app.py
```

## 七、项目结构

```text
Note/
├── app.py                   # 程序入口、应用协调及任务处理
├── storage.py               # SQLite 数据表、迁移、查询和事务
├── workspace.py             # 主窗口、侧栏和管理列表
├── desktop.py               # 桌面组件、拖动和提醒展示
├── calendar_page.py         # 日历及每日汇总
├── journal.py               # 情绪日记和四象限日记
├── ledger.py                # 收支记账和金额计算
├── category_manager.py      # 分类管理
├── note_preview.py          # 图片和便签预览
├── visual.py                # 公共图形界面组件
├── palette.py               # 颜色配置
├── assets/                  # 图标和 Logo
├── tests/                   # 自动化测试
├── docs/                    # 架构、使用和 GitHub 说明
├── requirements.txt         # 运行依赖
├── requirements-dev.txt     # 开发及打包依赖
├── setup.cmd                # 环境初始化
├── run.cmd                  # 程序启动
├── test.cmd                 # 自动化测试
└── build.cmd                # Windows 可执行文件构建
```

## 八、测试

准备好项目环境后，双击 `test.cmd`，或在 PowerShell 中运行：

```powershell
.\.venv\Scripts\python.exe -m unittest discover -s tests -v
```

测试使用临时数据库，不读取或清空默认用户数据库。部分测试会创建 Tk 窗口，因此需要 Windows 图形桌面环境。仓库还包含 Windows / Python 3.12 的 GitHub Actions 测试配置。

## 九、当前功能边界

- 当前为本地单用户应用，没有账号体系、云同步、协作或后台服务。
- 提醒依赖程序运行；程序完全退出后不会持续提供后台提醒。
- 数据库存储在本地，未进行加密，请用户自行备份并保护个人数据。
- 桌面组件目前面向 Windows 环境，暂未适配 macOS 和 Linux。

## 十、开源来源与版权说明

本项目源码包中包含原项目的 README、文档及相关代码。若将本项目公开发布，请保留原作者的版权声明，并遵守原始仓库所采用的许可证要求。

- 原始 GitHub 仓库：**[请填写原始仓库完整 URL]**
- 原始项目作者：**[请核实并填写]**
- 许可证：**[请根据原始仓库 LICENSE 文件核实后填写]**

在确认原始仓库及许可证之前，不要将本项目标注为完全原创，也不要自行添加与原项目许可不一致的授权声明。

## 参考资料

- [Python 3.12 文档](https://docs.python.org/3.12/)
- [Tkinter 文档](https://docs.python.org/3.12/library/tkinter.html)
- [SQLite 文档](https://docs.python.org/3.12/library/sqlite3.html)
- [Pillow 文档](https://pillow.readthedocs.io/)
