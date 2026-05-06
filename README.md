# PaddleOCR-json_PP-OCRv5 插件

**修改自 [win7_x64_PaddleOCR-json](https://github.com/hiroi-sora/Umi-OCR_plugins/tree/2.0.0/win7_x64_PaddleOCR-json)**

兼容 `Windows 7/10/11 x64`

**下载预编译好的插件: [Releases](https://github.com/OneDongua/PaddleOCR-json_PP-OCRv5_umi_plugin/releases/latest)**

## 部署步骤

### 第1步：克隆插件源码

```sh
git clone https://github.com/OneDongua/PaddleOCR-json_PP-OCRv5_umi_plugin.git
```

### 第2步：准备 PaddleOCR-json 可执行文件

#### 方式1：直接下载

- 浏览器访问 [PaddleOCR-json 发布页](https://github.com/OneDongua/PaddleOCR-json/releases) ，获取最新的 Windows 发行包 `PaddleOCR-json_v1.4.1-ext_windows_x64.7z` 的链接，下载压缩包并解压。
- 解压出来的文件夹，改名为 `win7_x64_PaddleOCR-json` 。

#### 方式2：从源码构建

- 见 [PaddleOCR-json Windows 构建指南](https://github.com/OneDongua/PaddleOCR-json/blob/main/cpp/README.md) 。

- 假设你完成了编译，那么将生成的所有可执行文件拷贝到一个 `win7_x64_PaddleOCR-json` 文件夹中。

### 第3步：组装插件，放置插件

- 将 `tools\Umi-OCR_plugins\win_linux_PaddleOCR-json` 中的所有文件，复制到 `win7_x64_PaddleOCR-json` 。
- 在 `win7_x64_PaddleOCR-json` 中，双击 `PaddleOCR-json.exe` 测试。正常情况下，应该打开一个控制台窗口，显示 `OCR init completed.` 。
- 将 `win7_x64_PaddleOCR-json` 整个文件夹，复制到 `UmiOCR-data\plugins` 中。

### 最终测试

启动 Umi-OCR ，测试各种功能吧。

在全局设置→拉到最底下，可以看到 PaddleOCR-json 插件相关的性能设置。

## 性能优化建议

PP-OCRv5 的精度更高，但模型更大、推理计算量更高，因此在默认 CPU 推理下通常会慢于旧版 PP-OCRv3 插件。为在不影响功能的前提下尽量提速，建议：

- 保持 `启用MKL-DNN加速` 为开启。
- `线程数` 不要盲目拉满，建议从 `8~16` 间测试最优值（本插件默认已限制上限 16，避免过多线程争抢）。
- 大图较多时可优先使用 `限制图像边长=960`（更快）或按精度需求改为 2880/4320；也支持 `自定义` 输入任意边长。
- 仅在确有旋转文本时开启 `纠正文本方向`，否则保持关闭以减少额外开销。
- 长时间批量识别时，按机器内存情况设置 `内存占用限制` 和 `内存闲时清理`，减少内存膨胀引起的性能抖动。

## 推理设备模式

插件新增了推理设备开关（全局设置）：

- `仅CPU`：强制使用 CPU 推理。
- `仅GPU`：强制使用 GPU 推理（要求你的 `PaddleOCR-json.exe` 为 GPU 版构建）。
- `CPU+GPU混合（推荐）`：优先尝试 GPU，若 GPU 初始化失败则自动回退到 CPU，稳定性更好。

可选参数：

- `GPU编号`：多卡环境下选择 GPU 序号（默认 0）。
- `GPU显存上限`：设置 OCR 进程使用的显存上限（MB）。

> 注意：如果你当前插件内的 `PaddleOCR-json.exe` 是 CPU-only 构建，选择 `仅GPU` 将无法启动；选择 `混合模式` 会自动回退到 CPU。
