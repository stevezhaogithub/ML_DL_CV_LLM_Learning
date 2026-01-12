# 高效使用 Jupyter Notebook 进阶教程

## 目录

1. [效率提升技巧](#效率提升技巧)
2. [高级魔法命令](#高级魔法命令)
3. [数据可视化优化](#数据可视化优化)
4. [调试与性能分析](#调试与性能分析)
5. [协作与版本控制](#协作与版本控制)
6. [自定义配置](#自定义配置)
7. [实用扩展推荐](#实用扩展推荐)

---

## 效率提升技巧

### 1. 多光标编辑

- **Mac**: `Cmd + 点击` 添加多个光标
- **Windows**: `Ctrl + 点击` 添加多个光标
- **Alt + 拖动**: 列选择模式

### 2. 快速查看文档

```python
# 方法一：Shift + Tab（在函数名后按）
pd.read_csv(  # 光标在这里按 Shift + Tab

# 方法二：使用 ? 查看文档
pd.read_csv?

# 方法三：使用 ?? 查看源代码
pd.read_csv??
```

### 3. 自动补全增强

```python
# Tab 补全不仅适用于代码，还适用于：
# - 文件路径: "../da" + Tab
# - 字典键: my_dict["ke" + Tab
# - 对象属性: df.col + Tab
```

### 4. 快速执行多个 Cell

```python
# Cell → Run All：运行所有 Cell
# Cell → Run All Above：运行当前 Cell 以上的所有 Cell
# Cell → Run All Below：运行当前 Cell 以下的所有 Cell
```

---

## 高级魔法命令

### 代码性能分析

```python
# 单行计时
%time result = sum(range(1000000))

# 多次运行取平均（更准确）
%timeit result = sum(range(1000000))

# 整个 Cell 计时
%%time
data = []
for i in range(1000):
    data.append(i ** 2)

# CPU 性能分析
%prun my_function()

# 内存分析（需要安装 memory_profiler）
%load_ext memory_profiler
%memit my_function()
```

### 变量与环境管理

```python
# 列出所有变量
%who

# 详细变量信息
%whos

# 删除变量
%reset -f  # 删除所有变量（慎用！）

# 保存变量到文件
%store my_variable
# 在新 session 中恢复
%store -r my_variable

# 查看命令历史
%history -n 1-10
```

### 跨语言支持

```python
# 运行 HTML
%%html
<div style="color: red; font-size: 20px;">
    这是 HTML 内容
</div>

# 运行 JavaScript
%%javascript
alert("Hello from JavaScript!");

# 运行 Bash
%%bash
echo "当前目录: $(pwd)"
ls -la

# 运行 LaTeX
%%latex
\begin{equation}
E = mc^2
\end{equation}
```

### 代码复用

```python
# 从文件加载代码到 Cell
%load my_script.py

# 运行外部脚本
%run my_script.py

# 运行并传递参数
%run my_script.py --input data.csv

# 将 Cell 内容写入文件
%%writefile my_module.py
def my_function():
    return "Hello World"
```

---

## 数据可视化优化

### 设置图表显示

```python
# 内联显示图表（默认）
%matplotlib inline

# 交互式图表（可缩放、平移）
%matplotlib widget  # 需要 ipympl

# 设置默认图表大小
import matplotlib.pyplot as plt
plt.rcParams['figure.figsize'] = [12, 6]
plt.rcParams['figure.dpi'] = 100

# 使用 Retina 显示（Mac 高清屏）
%config InlineBackend.figure_format = 'retina'
```

### Pandas 显示优化

```python
import pandas as pd

# 显示更多行和列
pd.set_option('display.max_rows', 100)
pd.set_option('display.max_columns', 50)
pd.set_option('display.width', None)

# 显示完整内容（不截断）
pd.set_option('display.max_colwidth', None)

# 浮点数显示格式
pd.set_option('display.float_format', '{:.2f}'.format)

# 一键恢复默认设置
pd.reset_option('all')
```

### 交互式组件

```python
from ipywidgets import interact, interactive, fixed
import ipywidgets as widgets

# 简单滑块交互
@interact(x=(0, 10, 1))
def square(x):
    print(f"{x}² = {x**2}")

# 多参数交互
@interact(
    a=widgets.IntSlider(min=0, max=10, value=5),
    b=widgets.Dropdown(options=['sin', 'cos', 'tan']),
    c=widgets.Checkbox(value=True, description='显示网格')
)
def plot_function(a, b, c):
    # 绑定参数的绘图代码
    pass
```

---

## 调试与性能分析

### 交互式调试

```python
# 方法一：使用 %debug（在报错后运行）
%debug

# 方法二：设置断点
from IPython.core.debugger import set_trace

def my_function(x):
    result = x * 2
    set_trace()  # 程序会在这里暂停
    return result + 1

# 方法三：自动进入调试模式
%pdb on  # 出错时自动进入调试器
```

### 性能分析

```python
# 行级性能分析
%load_ext line_profiler

def slow_function():
    total = 0
    for i in range(10000):
        total += i ** 2
    return total

%lprun -f slow_function slow_function()

# 内存使用分析
%load_ext memory_profiler
%mprun -f slow_function slow_function()
```

### 错误处理技巧

```python
# 忽略警告
import warnings
warnings.filterwarnings('ignore')

# 只忽略特定警告
warnings.filterwarnings('ignore', category=DeprecationWarning)

# 详细异常信息
%xmode Verbose  # 选项: Plain, Context, Verbose
```

---

## 协作与版本控制

### Git 集成

```python
# 在 Notebook 中使用 Git
!git status
!git add .
!git commit -m "更新分析结果"
!git push
```

### 清理输出后再提交

```bash
# 安装 nbstripout（清除输出后再提交）
pip install nbstripout

# 在仓库中设置自动清理
nbstripout --install
```

### 转换格式

```bash
# 转换为 Python 脚本
jupyter nbconvert --to script notebook.ipynb

# 转换为 HTML（含输出）
jupyter nbconvert --to html notebook.ipynb

# 转换为 PDF
jupyter nbconvert --to pdf notebook.ipynb

# 转换为 Markdown
jupyter nbconvert --to markdown notebook.ipynb

# 批量转换
jupyter nbconvert --to html *.ipynb
```

### 参数化执行（自动化）

```bash
# 使用 papermill 参数化运行 Notebook
pip install papermill

# 命令行执行
papermill input.ipynb output.ipynb -p param1 value1 -p param2 value2
```

---

## 自定义配置

### 生成配置文件

```bash
jupyter notebook --generate-config
# 配置文件位置: ~/.jupyter/jupyter_notebook_config.py
```

### 常用配置选项

```python
# 编辑 ~/.jupyter/jupyter_notebook_config.py

# 设置默认工作目录
c.NotebookApp.notebook_dir = '/path/to/your/notebooks'

# 禁用自动打开浏览器
c.NotebookApp.open_browser = False

# 设置默认端口
c.NotebookApp.port = 8888

# 允许远程访问
c.NotebookApp.ip = '0.0.0.0'

# 设置密码（使用之前生成的密码哈希）
c.NotebookApp.password = 'sha1:xxx...'

# 关闭 Token 认证（不推荐用于公共环境）
c.NotebookApp.token = ''
```

### 自定义快捷键

```javascript
// 在浏览器中按 Esc 进入命令模式，然后按 H 查看所有快捷键

// 自定义快捷键：编辑 ~/.jupyter/custom/custom.js
Jupyter.keyboard_manager.command_shortcuts.add_shortcut('ctrl-k', {
    handler: function() {
        Jupyter.notebook.move_cell_up();
        return false;
    }
});
```

### 自定义 CSS 样式

```css
/* 创建 ~/.jupyter/custom/custom.css */

/* 增大代码字体 */
.CodeMirror {
    font-size: 14px;
    line-height: 1.5;
}

/* 修改 Cell 宽度 */
.container {
    width: 90% !important;
}

/* 修改输出区域背景 */
.output_area {
    background-color: #f9f9f9;
}
```

---

## 实用扩展推荐

### 安装 nbextensions

```bash
pip install jupyter_contrib_nbextensions
jupyter contrib nbextension install --user

# 启用配置页面
pip install jupyter_nbextensions_configurator
jupyter nbextensions_configurator enable --user
```

### 推荐扩展列表

| 扩展名 | 功能 |
|--------|------|
| **Table of Contents (2)** | 自动生成目录导航 |
| **Collapsible Headings** | 可折叠的标题区域 |
| **ExecuteTime** | 显示 Cell 执行时间 |
| **Variable Inspector** | 变量查看器（类似 IDE） |
| **Codefolding** | 代码折叠 |
| **AutoSaveTime** | 显示自动保存时间 |
| **Hinterland** | 自动代码补全 |
| **Scratchpad** | 临时代码测试区 |
| **Code prettify** | 代码格式化 |
| **Notify** | 长时间运行完成后通知 |

### 启用扩展

```bash
# 命令行启用
jupyter nbextension enable toc2/main
jupyter nbextension enable collapsible_headings/main
jupyter nbextension enable execute_time/ExecuteTime
jupyter nbextension enable varInspector/main
```

---

## 快速参考卡片

### 最常用快捷键

| 操作 | 快捷键 |
|------|--------|
| 运行 Cell | `Shift + Enter` |
| 运行并停留 | `Ctrl + Enter` |
| 上方插入 Cell | `A` |
| 下方插入 Cell | `B` |
| 删除 Cell | `D, D` |
| 转为代码 | `Y` |
| 转为 Markdown | `M` |
| 保存 | `Ctrl/Cmd + S` |
| 查找替换 | `Ctrl/Cmd + Shift + F` |
| 命令面板 | `Ctrl/Cmd + Shift + P` |

### 最常用魔法命令

| 命令 | 用途 |
|------|------|
| `%time` | 单次计时 |
| `%timeit` | 多次计时取平均 |
| `%who` | 列出变量 |
| `%run` | 运行脚本 |
| `%load` | 加载代码 |
| `%debug` | 进入调试 |
| `%%writefile` | 写入文件 |

---

**掌握这些技巧，让你的 Jupyter 效率提升 10 倍！** 🚀