# iStoreOS N1 Kernel Build

用于 Phicomm N1（Amlogic S905D）iStoreOS 内核配置验证与后续构建。

## 当前阶段：只验证配置，不编译、不刷机

GitHub Actions 工作流 `.github/workflows/prepare-config.yml` 会：

1. 检出官方 `istoreos/istoreos` 的 `istoreos-25.12` 分支（可在手动运行时指定 ref）。
2. 下载 KoolCenter 当前发布的 N1 `config.seed`。
3. 启用 OpenWrt 构建配置中的：
   - `CONFIG_KERNEL_FTRACE=y`
   - `CONFIG_KERNEL_FUNCTION_TRACER=y`
   - `CONFIG_KERNEL_DYNAMIC_FTRACE=y`
4. 运行 `make defconfig`，确认三个选项没有被依赖关系清除。
5. 上传 `n1-kernel-config-preview` 配置预览 artifact。

**此工作流不会编译内核、生成固件或对设备进行任何操作。**

## 运行方式

1. 打开仓库的 **Actions** 页面。
2. 选择 **Prepare N1 kernel config**。
3. 点击 **Run workflow**。
4. 首次运行保持默认分支 `istoreos-25.12`。
5. 工作流完成后检查日志，并下载 `n1-kernel-config-preview` artifact。

## 安全说明

当前阶段只验证 OpenWrt 顶层构建配置。确认成功后，还需要在后续构建阶段检查最终 Linux kernel `.config` 中的 `CONFIG_FUNCTION_TRACER` 和 `CONFIG_DYNAMIC_FTRACE`，并验证内核版本、设备树和模块 ABI。未完成这些检查前，不应将产物安装到 N1。

## 设备基线

- 设备：Phicomm N1 / Amlogic S905D
- iStoreOS：25.12.6
- Build ID：2026110611
- Target：amlogic/meson
- Architecture：aarch64_cortex-a53
- 当前运行内核：6.12.112
