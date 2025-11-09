# 使用 GitHub Actions 编译 PyTorch3D

## 方法说明

本地编译 PyTorch3D 失败是因为系统 CUDA 头文件冲突。使用 GitHub Actions 可以在干净的云端环境中编译，避免本地环境问题。

## 使用步骤

### 方式1: 使用当前项目（推荐）

1. **初始化 git 仓库并推送到 GitHub**
   ```bash
   cd /datassd/home/yanmengcheng/cadrille
   git init
   git add .github/workflows/build_pytorch3d.yml
   git commit -m "Add GitHub Actions workflow for PyTorch3D"

   # 在 GitHub 上创建一个新仓库，然后：
   git remote add origin https://github.com/你的用户名/你的仓库名.git
   git branch -M main
   git push -u origin main
   ```

2. **在 GitHub 网页上触发编译**
   - 访问你的仓库页面
   - 点击 "Actions" 标签
   - 左侧选择 "Build PyTorch3D Wheel"
   - 点击 "Run workflow" 按钮
   - 等待编译完成（约10-15分钟）

3. **下载编译好的 wheel 文件**
   - 编译完成后，在 workflow 运行页面下方找到 "Artifacts"
   - 下载 `pytorch3d-wheel` 压缩包
   - 解压得到 .whl 文件

4. **本地安装**
   ```bash
   conda activate test
   pip install --no-deps /path/to/downloaded/pytorch3d-*.whl
   python -c "import pytorch3d; print(f'✓ PyTorch3D {pytorch3d.__version__} 安装成功')"
   ```

### 方式2: 使用临时仓库

如果不想在项目中添加 .github 目录，可以创建临时仓库：

1. **创建临时目录并推送**
   ```bash
   mkdir -p /tmp/pytorch3d-builder
   cp -r /datassd/home/yanmengcheng/cadrille/.github /tmp/pytorch3d-builder/
   cd /tmp/pytorch3d-builder
   git init
   git add .
   git commit -m "PyTorch3D builder"

   # 在 GitHub 创建临时仓库后推送
   git remote add origin https://github.com/你的用户名/pytorch3d-builder.git
   git push -u origin main
   ```

2. 然后按照方式1的步骤2-4操作

## 注意事项

1. **GitHub Actions 免费额度**
   - 公开仓库：无限制
   - 私有仓库：每月 2000 分钟免费额度
   - 此编译大约需要 10-15 分钟

2. **编译的 wheel 包规格**
   - Python 3.10
   - PyTorch 2.5.1
   - CUDA 12.4
   - 与你当前 conda 环境完全匹配

3. **Artifact 保留期**
   - 编译产物保留 7 天
   - 下载后请妥善保存

## 替代方案

如果不方便使用 GitHub，也可以：

1. **使用 Google Colab** - 免费 GPU 环境编译
2. **向项目维护者请求预编译包** - 在 PyTorch3D 项目提 issue
3. **降级到 PyTorch 1.13** - 使用 conda 提供的预编译 PyTorch3D

## 故障排除

如果 GitHub Actions 编译失败：
- 检查 Actions 日志中的具体错误
- 可能需要调整 CUDA 版本或依赖项
- 可以在 workflow 文件中修改配置后重新运行
