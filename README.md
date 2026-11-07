# DualSense Wireless Enhance

面向 Windows 的 DualSense 蓝牙反馈工具。游戏可以通过虚拟 DualSense 发送原有的自适应扳机和精细触觉，也可以通过虚拟 Xbox 360 发送普通震动。手柄通过电脑的蓝牙连接。

## 选择模式

| 模式 | 适用情况 | 握把与扳机 |
| --- | --- | --- |
| 原生增强（Native enhancement） | PC 游戏支持有线 DualSense 高级反馈 | 转发游戏实际发送的触觉、普通震动和 L2/R2 扳机指令 |
| Xbox 兼容（Xbox compatibility） | 游戏支持 Xbox 360 / XInput | 转发游戏震动，可选将震动转成触觉，或设置左右扳机的阻力预设 |
| 蓝牙直通（Bluetooth direct） | 游戏能直接识别实体手柄，或需要排查双输入 | 恢复实体手柄输入，可在触觉页面选择音频震动或扳机预设 |

Xbox 普通震动只含轻重两路强度。扳机预设由用户选定。音频触觉从选定的 Windows 播放设备生成，设备上的其他应用声音也可能触发震动。按游戏进程独立捕获留待后续实现。

## 开始使用

1. 在 Windows 11 x64 中按住手柄的 PS 键和创建键，直到灯条闪烁，再从蓝牙设置中配对。先退出其他接管同一手柄的工具。
2. 运行 `dualsense-wireless-enhance.exe`。在 Overview 的 Game feedback 中选模式。原生增强和 Xbox 兼容需要 [usbip-win2](https://github.com/vadimgrn/usbip-win2/releases/tag/v.0.9.8.1)。出现双重输入时，在工具内启用 [HidHide](https://github.com/nefarius/HidHide/releases) 来隐藏实体手柄。安装驱动后按安装器提示重启。
3. 确认 Input device 显示预期的设备后再启动游戏。用原生增强时，如果 Steam Input 接管了手柄，可在该游戏的 Steam 属性中关闭 Steam Input，再重新打开游戏。若游戏启动时未看到所选设备，也先确认模式和设备状态，再重启游戏。
4. Game feedback 会显示是否收到了游戏震动、原生触觉或扳机指令。测试普通震动、左右握把可使用 Overview 的 Rumble test、Haptic L 和 Haptic R。用 Adaptive triggers 调整扳机阻力，用 Haptics & audio 调整触觉强度。蓝牙直通下的原生输出由游戏直接发送，工具面板显示的是经过工具的信号。
5. 在 Profiles & games 中可按游戏的可执行文件保存配置。自动识别仅提供候选模式，可以手动指定。切换模式后若游戏不再识别手柄，先退出游戏再切换。选择 Bluetooth direct 会撤销当前虚拟输入，实体设备会重新可见。要完全交还反馈控制，可在该模式下点击 Release controller and quit。

首版的反馈模式继承了上游的传输实现。每款游戏能提供的反馈取决于版本、输入模式和设置。可先用《最后生还者 Part I》对照蓝牙直连和虚拟 DualSense，再用支持 Xbox 震动的游戏测试兼容模式。反馈触感和跨游戏覆盖需要在实体手柄上验证。

## 下载与构建

在 GitHub 的 **Actions → Windows build** 中打开对应成功的运行，下载 `dualsense-wireless-enhance-windows-x64` 构建产物。使用 Rust stable MSVC、Visual Studio C++ 构建工具和 Windows SDK 可在本地构建。

```powershell
cargo test --workspace --locked
cargo build --release --locked -p dsz
```

可执行文件位于 `target/release/dualsense-wireless-enhance.exe`。配置保存在 `%APPDATA%\DualSense Wireless Enhance`。在程序旁创建 `dualsense-wireless-enhance-data` 文件夹可使用便携配置。

## 上游与许可

以 [DualSense Zen 0.4.5](https://github.com/MaximumFreak/dsz/tree/v0.4.5) 为底座，保留原始提交历史、[MIT 许可](LICENSE) 和第三方声明。[上游协议来源与致谢](https://github.com/MaximumFreak/dsz/blob/v0.4.5/README.md#protocol-sources) 列出了手柄协议的研究和实现。DualSense 和 PlayStation 是 Sony Interactive Entertainment 的商标。
