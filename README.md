# Android GKI Kernel for Xiaomi POCO F5 (marble)

Custom Linux 5.10 GKI (Google Kernel Image) kernel source tree for Xiaomi POCO F5 (`marble` / Snapdragon 7+ Gen 2), optimized for low latency, high throughput, and full GKI ABI compatibility.

---

## 🛠 Features & Architecture Highlights

### Kernel Infrastructure & Compilation
- **Toolchain**: Clang 17 / LLVM ThinLTO (`CONFIG_LTO_CLANG_THIN=y`).
- **GKI ABI Preservation**: `CONFIG_SLUB_DEBUG=y` and `CONFIG_UNMAP_KERNEL_AT_EL0=y` fully maintained for 100% ABI compatibility with stock Xiaomi vendor modules (`msm_kgsl`, `wlan`, `camera`).
- **Out-of-Tree Driver Support**:
  - Unconditional export of `__stack_chk_guard` in `arch/arm64/kernel/process.c`.
  - CRC version check bypass in `kernel/module.c` (`check_version()`).
  - Control Flow Integrity (CFI) trap handler bypass in `kernel/cfi.c` (`cfi_slowpath_handler()`).

### Networking & Schedulers
- **TCP Congestion Control**: TCP BBR enabled (`CONFIG_TCP_CONG_BBR=y`) with Fair Queue (`CONFIG_NET_SCH_FQ=y`) packet pacing.
- **CPU Scheduling**: Utilization Clamping (`CONFIG_UCLAMP_TASK=y`, `CONFIG_UCLAMP_TASK_GROUP=y`) enabled for instant CPU frequency scaling on foreground workloads.

---

## 📦 Building

This kernel is built automatically using GitHub Actions.

### Manual Build Requirements
- Clang 17.0.6 or higher (Android NDK r26 / AOSP Clang).
- `aarch64-linux-android-` cross compiler toolchain.

```bash
make ARCH=arm64 O=out marble_defconfig
make ARCH=arm64 O=out -j$(nproc)
```

---

## 📄 License
This project is licensed under the terms of the GNU General Public License v2.0 (GPL-2.0).
