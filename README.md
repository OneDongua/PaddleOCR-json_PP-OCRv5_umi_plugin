# PaddleOCR-json_PP-OCRv5 插件

**修改自 [win7_x64_PaddleOCR-json](https://github.com/hiroi-sora/Umi-OCR_plugins/tree/2.0.0/win7_x64_PaddleOCR-json)**

兼容 `Windows 7/10/11 x64`

**下载预编译好的插件: [Releases](https://github.com/OneDongua/PaddleOCR-json_PP-OCRv5_umi_plugin/releases/latest)**

## 相比原版插件的改进

- **模型升级**：将 PaddleOCR 从 v2.6/v2.8 升级至 v3.1（PP-OCRv5），识别精度大幅提升。
- **快速模型**：新增 PP-OCRv5 mobile_rec 轻量识别模型，速度提升 3~5 倍，精度仍高于旧版 v3。
- **多语言分离**：语言选项分为简体中文、繁體中文、English、日本語，各有高精度/快速两种模式。
- **推理设备模式**：新增仅CPU、仅GPU、CPU+GPU混合三种推理模式。
- **TensorRT 加速**：支持启用 TensorRT 加速 GPU 推理。
- **FP16 精度**：支持 FP16 推理精度，可加速 GPU 推理。
- **文本检测开关**：可关闭 det 检测以加速单行文本识别。
- **识别批处理数**：可调整 rec_batch_num 提高吞吐量。
- **竖排文字模式**：可按竖排阅读顺序重排识别结果（从右到左逐列，每列从上到下）。
- **参数传递修复**：修复启动参数传递方式，避免含空格路径的解析错误。
- **字典文件修复**：使用正确的 PP-OCRv5 字典（18383 字符），替代旧版 v1 字典（245 字符）。

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

### 第4步：下载快速模型（可选）

如需使用"快速"模式，需额外下载 PP-OCRv5 mobile_rec 模型：

1. 从 HuggingFace 下载以下 3 个文件：
   - [inference.pdiparams](https://huggingface.co/PaddlePaddle/PP-OCRv5_mobile_rec/resolve/main/inference.pdiparams)（约 16 MB，模型权重）
   - [inference.json](https://huggingface.co/PaddlePaddle/PP-OCRv5_mobile_rec/resolve/main/inference.json)（模型结构）
   - [inference.yml](https://huggingface.co/PaddlePaddle/PP-OCRv5_mobile_rec/resolve/main/inference.yml)（模型配置）
2. 在插件的 `models/` 目录下创建 `PP-OCRv5_mobile_rec_infer` 文件夹。
3. 将下载的 3 个文件放入该文件夹中。

最终目录结构：

```
models/
├── configs.txt
├── config_simplified_server.txt
├── config_simplified_fast.txt
├── config_traditional_server.txt
├── config_traditional_fast.txt
├── config_en_server.txt
├── config_en_fast.txt
├── config_ja_server.txt
├── config_ja_fast.txt
├── ppocr_keys_v5.txt
├── PP-OCRv5_mobile_det_infer/
├── PP-OCRv5_mobile_rec_infer/        ← 需手动下载
│   ├── inference.pdiparams
│   ├── inference.json
│   └── inference.yml
├── PP-OCRv5_server_rec_infer/
└── PP-LCNet_x0_25_textline_ori_infer/
```

### 最终测试

启动 Umi-OCR ，测试各种功能吧。

在全局设置→拉到最底下，可以看到 PaddleOCR-json 插件相关的性能设置。

## 全局设置说明
**注：本插件目前仅支持CPU模式，GPU模型现不可用，请在设置中设置为“仅CPU”模式使用！！！**

| 设置项           | 默认值 | 说明                                          |
| ---------------- | ------ | --------------------------------------------- |
| 推理设备模式     | 仅CPU  | 仅CPU / 仅GPU / CPU+GPU混合（推荐）           |
| GPU编号          | 0      | 多卡环境下选择 GPU 序号                       |
| 启用MKL-DNN加速  | 开启   | 大幅加快 CPU 推理速度，但增加内存占用         |
| 线程数           | 自动   | CPU 推理线程数，建议 8~16 间测试最优值        |
| 启用TensorRT加速 | 关闭   | 加速 GPU 推理，需 GPU 版 exe 及 TensorRT 环境 |
| 推理精度         | FP32   | FP16 可加速 GPU 推理，可能略微降低精度        |
| 内存占用限制     | 自动   | 引擎内存超限时执行清理                        |
| 内存闲时清理     | 60秒   | 引擎空闲超时后执行清理                        |

## 局部设置说明

| 设置项       | 默认值             | 说明                                   |
| ------------ | ------------------ | -------------------------------------- |
| 语言/模型库  | 简体中文（高精度） | 高精度用 server_rec，快速用 mobile_rec |
| 启用文本检测 | 开启               | 单行文本可关闭以加速                   |
| 纠正文本方向 | 关闭               | 识别倾斜/倒置文本，可能降低速度        |
| 识别批处理数 | 6                  | 增大可提高吞吐量，增加内存/显存占用    |
| 竖排文字模式 | 关闭               | 按竖排阅读顺序重排结果（从右到左逐列） |
| 限制图像边长 | 960                | 压缩大图加速，可能降低精度             |

## 语言/模型库选项

| 选项               | rec 模型          | 精度 | 速度 |
| ------------------ | ----------------- | ---- | ---- |
| 简体中文（高精度） | server_rec (80MB) | 最高 | 较慢 |
| 简体中文（快速）   | mobile_rec (16MB) | 较高 | 快   |
| 繁體中文（高精度） | server_rec        | 最高 | 较慢 |
| 繁體中文（快速）   | mobile_rec        | 较高 | 快   |
| English（高精度）  | server_rec        | 最高 | 较慢 |
| English（快速）    | mobile_rec        | 较高 | 快   |
| 日本語（高精度）   | server_rec        | 最高 | 较慢 |
| 日本語（快速）     | mobile_rec        | 较高 | 快   |

> PP-OCRv5 的 rec 模型本身是多语言的，同时支持简体中文、繁体中文、英文和日文。不同语言选项使用相同的模型和字典，区别仅在于标签分类。

## 推理设备模式
**注：本插件目前仅支持CPU模式，GPU模型现不可用，请在设置中设置为“仅CPU”模式使用！！！**
- `仅CPU`：强制使用 CPU 推理。
- `仅GPU`：强制使用 GPU 推理（要求 `PaddleOCR-json.exe` 为 GPU 版构建）。
- `CPU+GPU混合（推荐）`：优先尝试 GPU，若 GPU 初始化失败则自动回退到 CPU，稳定性更好。

> 注意：如果你当前插件内的 `PaddleOCR-json.exe` 是 CPU-only 构建，选择 `仅GPU` 将无法启动；选择 `混合模式` 会自动回退到 CPU。

## 性能优化建议

PP-OCRv5 的精度更高，但模型更大、推理计算量更高，因此在默认 CPU 推理下通常会慢于旧版 PP-OCRv3 插件。为在不影响功能的前提下尽量提速，建议：

- 保持 `启用MKL-DNN加速` 为开启。
- `线程数` 不要盲目拉满，建议从 `8~16` 间测试最优值（本插件默认已限制上限 16，避免过多线程争抢）。
- 使用"快速"模型（mobile_rec），速度提升 3~5 倍，精度仍高于旧版 v3。
- 大图较多时可优先使用 `限制图像边长=960`（更快）或按精度需求改为 2880/4320；也支持 `自定义` 输入任意边长。
- 仅在确有旋转文本时开启 `纠正文本方向`，否则保持关闭以减少额外开销。
- 单行文本可关闭 `启用文本检测` 以跳过检测阶段，显著加速。
- 长时间批量识别时，按机器内存情况设置 `内存占用限制` 和 `内存闲时清理`，减少内存膨胀引起的性能抖动。
