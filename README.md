# PaddleOCR-json_PP-OCRv5 插件

**修改自 [win7_x64_PaddleOCR-json](https://github.com/hiroi-sora/Umi-OCR_plugins/tree/2.0.0/win7_x64_PaddleOCR-json)**

兼容 `Windows 7/10/11 x64`

下载预编译好的插件: [Releaese](https://github.com/OneDongua/PaddleOCR-json_PP-OCRv5_umi_plugin/releases/latest)

## 部署步骤

### 第1步：克隆插件源码

```sh
git clone https://github.com/OneDongua/PaddleOCR-json_PP-OCRv5_umi_plugin.git
```

### 第2步：准备 PaddleOCR-json 可执行文件

#### 方式1：直接下载

- 浏览器访问 [PaddleOCR-json 发布页](https://github.com/OneDongua/PaddleOCR-json/releases) ，获取最新的 Windows 发行包 `PaddleOCR-json_v1.X.X_windows_x86-64.7z` 的链接，下载压缩包并解压。
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
