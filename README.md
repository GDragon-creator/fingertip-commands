# bug-free-memory
基于MediaPipe：指尖命令
**注意:**
*   `languages.json` 是程序运行所必需的。
*   `fonts/simhei.ttf` (或其他中文字体) 如果存在，将用于在摄像头画面上渲染中文提示。如果找不到，程序会尝试使用 `arial.ttf`，若仍失败则提示字体缺失。
*   `images/wechat.png` 是可选的，用于 "联系我们" 菜单项中的二维码展示。

## 🚀 安装与运行

1.  **克隆或下载仓库:**
    ```bash
    git clone https://github.com/GDragon-creator/bug-free-memory.git
    ```
    或者直接下载 ZIP 文件并解压。

2.  **安装依赖:**
    打开终端或命令提示符，进入项目目录，然后运行：
    ```bash
    pip install -r requirements.txt
    ```
    或者逐个安装上面 "前置条件" 中列出的库。

3.  **准备必要文件:**
    *   确保 `languages.json` 文件与 `automated_mediaplayer.py` 在同一目录下。
    *   (可选但推荐) 创建 `fonts` 文件夹，并将中文字体文件 (如 `simhei.ttf`) 放入其中。
    *   (可选) 创建 `images` 文件夹，并将 `wechat.png` (或其他联系方式图片) 放入其中。

4.  **运行程序:**
    ```bash
    python automated_mediaplayer.py
    ```

## 🛠 使用说明

### 1. 配置手势绑定 (GUI 界面)

程序启动后会显示配置界面：

*   **界面布局:**
    *   顶部是菜单栏（选项、语言、关于）。
    *   主体部分列出了 "1 指手势" 到 "5 指手势" 的配置行。
    *   每行显示：手势名称、当前绑定的动作（或 "未设置"）、"设置动作" / "取消动作" 按钮。
    *   底部是状态提示和 "完成配置并开始" 按钮。

*   **设置动作:**
    1.  点击对应手指数量（例如 "3 指手势"）旁边的 **"设置动作"** 按钮。
    2.  状态栏会提示 "正在为 X 指手势捕获输入..."，同时该行的动作描述变为 "捕获中..."。
    3.  此时，你可以执行想要绑定的操作：
        *   **单个按键:** 按下并释放一个键 (如 `空格`, `A`, `F5`)。
        *   **组合键:** 按住修饰键 (如 `Ctrl`, `Alt`, `Shift`) 再按其他键 (如 `Ctrl + C`)，然后依次释放。
        *   **鼠标滚动:** 向上或向下滚动鼠标滚轮。
        *   **鼠标点击:** 点击鼠标左键、右键或中键（注意：避免点击到界面上的 "设置动作" 按钮本身）。
    4.  操作被捕获后，该行的动作描述会更新为捕获到的操作（例如 "按键: space"，"组合键: ctrl+c"，"向上滚动"），按钮变为 **"取消动作"**。
    5.  重复此过程为1到5根手指设置你需要的动作。

*   **取消动作:**
    如果想清除某个手指的绑定，点击其旁边的 **"取消动作"** 按钮。

*   **菜单栏:**
    *   **选项:**
        *   `保存配置`: 手动将当前绑定保存到 `settings.json`。
        *   `导出配置`: 将当前绑定另存为一个 `.json` 文件。
        *   `导入配置`: 从一个 `.json` 文件加载绑定。
        *   `重置配置`: 清除所有已设置的绑定。
        *   `退出`: 关闭配置界面并退出程序。
    *   **语言:**
        *   选择界面语言 (自动、简体中文、繁体中文、English)。选择后界面文本会立即更新，并且偏好会保存到 `config.ini`。
    *   **关于:**
        *   `介绍`: 显示程序的基本信息。
        *   `联系我们`: 显示联系方式（如果 `images/wechat.png` 存在，会尝试显示二维码）。

### 2. 开始手势控制

1.  完成所有或部分手势绑定后，点击界面底部的 **"完成配置并开始"** 按钮。
    *   如果存在未设置的手势，程序会弹窗确认是否继续。
2.  配置界面关闭，一个新的窗口（摄像头画面）打开，标题为 "Gesture Control"。

### 3. 手势控制模式

*   **摄像头画面:**
    *   实时显示你的手部以及 MediaPipe 绘制的关键点。
    *   左上角会显示：
        *   当前识别到的手指数量。
        *   识别到的手是左手还是右手（或未知）。
        *   如果双手同时举起，会显示退出倒计时。
*   **执行动作:**
    *   将手放在摄像头前，伸出对应数量的手指（1-5根）。
    *   保持手势稳定一小段时间 (约 0.25 秒，以防误触)。
    *   程序会执行你为该手指数量绑定的操作。
    *   两次动作之间有短暂的冷却时间 (约 0.4 秒)。
*   **特殊操作:**
    *   **退出程序:**
        *   在摄像头画面窗口按 `ESC` 键。
        *   或者，同时将两只手举在摄像头前并保持约3秒，程序会自动退出。
    *   **重新配置:**
        *   在摄像头画面窗口按 `R` 或 `r` 键。程序会关闭摄像头窗口，清空当前临时配置（但 `settings.json` 中的配置不变），然后重新打开GUI配置界面。你可以重新加载、修改或从头开始配置。

## 💡 注意事项与故障排除

*   **摄像头:** 确保你的电脑连接了摄像头并且驱动正常。如果程序无法打开摄像头，会提示错误。
*   **光线与背景:** 良好的光线条件和相对简洁的背景有助于提高手势识别的准确性。
*   **字体:** 为了在摄像头画面正确显示中文提示，建议在项目根目录下创建 `fonts` 文件夹，并放入 `simhei.ttf` (或其他支持中文的 `.ttf` 字体)。
*   **`languages.json` 文件:** 此文件必须存在且与主脚本在同一目录，否则程序无法启动。
*   **拇指识别:** 拇指的开合判断相对复杂，可能会因角度和手型略有差异。
*   **动作冲突:** 一个动作（如 "按键: A"）只能绑定到一个手势数量。如果你将一个已绑定的动作设置给另一个手势，前一个手势的该绑定会被自动清除。
