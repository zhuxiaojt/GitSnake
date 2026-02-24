# GitSnake

GitSnake 是一个自动化工具，可基于 GitHub 贡献图生成贪吃蛇动画，并通过 GitHub Pages 实现自动部署与访问，无需手动干预即可每日同步最新贡献数据。

## 核心功能

1. 自动化更新：每日 UTC 时间 0 点（北京时间 8 点）自动执行，同步最新 GitHub 贡献数据生成动画；
2. 多格式输出：默认生成 6 类动画文件，包含基础样式、深色模式样式及蓝色定制配色样式，覆盖 SVG 静态格式与 GIF 动态格式；
3. 一键部署：生成的动画文件自动部署至 GitHub Pages，可直接通过固定链接访问；
4. 高度可定制：支持自定义动画配色、新增输出文件类型，满足个性化需求。

## 使用方法

### 1. 仓库复刻

点击仓库右上角「Fork」按钮，将本仓库复刻至个人 GitHub 账号下，仓库名称默认保留为 GitSnake 即可。

### 2. 分支创建

在复刻后的仓库中，创建名为 `output` 的空分支（无需提交任何文件），用于承载 GitHub Pages 部署的动画文件。

### 3. GitHub Pages 配置

1. 进入仓库 `Settings` 页面，找到 `Pages` 选项；
2. 在 `Build and deployment` 区域，将 `Source` 设置为 `Deploy from a branch`；
3. `Branch` 选择 `output` 分支，路径选择 `/(root)`，点击「Save」保存配置。

### 4. 动画生成与访问

- 自动触发：配置完成后，每日 UTC 时间 0 点会自动执行工作流生成并部署动画；
- 手动触发：如需立即生成，可进入仓库「Actions」页面，选择「Generate snake animation」，点击「Run workflow」手动执行；
- 访问链接：部署完成后，可通过以下固定链接访问生成的动画文件（替换 `<your-username>` 为个人 GitHub 用户名）：
  - 基础样式 SVG：`https://<your-username>.github.io/GitSnake/github-snake.svg`
  - 基础样式 GIF：`https://<your-username>.github.io/GitSnake/github-snake.gif`
  - 深色模式 SVG：`https://<your-username>.github.io/GitSnake/github-snake-dark.svg`
  - 深色模式 GIF：`https://<your-username>.github.io/GitSnake/github-snake-dark.gif`
  - 蓝色配色 SVG：`https://<your-username>.github.io/GitSnake/ocean.svg`
  - 蓝色配色 GIF：`https://<your-username>.github.io/GitSnake/ocean.gif`

## 自定义配置说明

可通过修改仓库 `.github/workflows/generate-snake.yml` 文件中的配置项，实现动画样式、输出文件的自定义调整，核心配置项为 `outputs` 字段，具体说明如下。

### 1. 配置文件路径

编辑 `.github/workflows/generate-snake.yml`，找到 `steps` 下「generate snake.svg」步骤的 `outputs` 参数，该参数内每一行对应一个输出文件。

### 2. 新增输出文件

如需新增动画文件，只需在 `outputs` 中新增一行配置，格式为「文件名称?自定义参数」，示例配置如下：

```
普通的SVG格式示例：dist/github-snake.svg
普通的GIF格式示例：dist/github-snake.gif
深色的SVG格式示例：dist/github-snake-dark.svg?palette=github-dark
深色的GIF格式示例：dist/github-snake-dark.gif?palette=github-dark
自定义SVG格式示例：dist/ocean.svg?color_snake=orange&color_dots=#bfd6f6,#8dbdff,#64a1f4,#4b91f1,#3c7dd9
自定义GIF格式示例：dist/ocean.gif?color_snake=orange&color_dots=#bfd6f6,#8dbdff,#64a1f4,#4b91f1,#3c7dd9
```

### 3. 自定义配色

#### （1）预设配色方案

使用 `palette` 参数可快速调用官方预设配色，支持值如下：

- `github`：默认浅色配色（无需显式声明）；

- `github-dark`：深色模式配色；

- `github-light`：浅色模式配色（与 github 一致）。

示例配置：dist/github-snake-dark.svg?palette=github-dark

#### （2）自定义蛇身与圆点颜色

- `color_snake`：设置贪吃蛇主体颜色，支持十六进制（#FF0000）、英文名称（red）等格式；

- `color_dots`：设置贡献圆点颜色，需按顺序填写 5 种颜色（用逗号分隔），对应「0 贡献、低贡献、中低贡献、中高贡献、高贡献」，仅支持十六进制格式。

蓝色配色示例配置：dist/ocean.svg?color_snake=orange&color_dots=#bfd6f6,#8dbdff,#64a1f4,#4b91f1,#3c7dd9

### 4. 配置生效

修改配置后提交至 `main` 分支，工作流会自动触发执行，也可手动触发「Generate snake animation」工作流，新配置将同步生效。
