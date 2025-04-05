GN 是一种专门生成 Ninja 文件的工具，主要用于 Google 的项目中，而 CMake 则是一个更加通用的构建系统生成器，可以生成多种构建系统文件，包括 Ninja 文件。这三者在构建系统中各有其用途和优势。

GN（Generate Ninja）

- **用途**：GN 是一个生成 Ninja 构建文件的工具，最初是由 Google 为 Chromium 项目开发的。
- **特点**：GN 通过定义 `.gn` 文件和 `BUILD.gn` 文件来描述构建过程，然后生成 Ninja 构建文件。
- **优点**：与手动编写 Ninja 文件相比，GN 提供了更高层次的抽象，简化了构建文件的编写和维护。提供了工具查询模块依赖图谱。

Ninja

- **用途**：Ninja 是一个专注于速度的小型构建系统，主要用于快速执行增量构建。
- **特点**：Ninja 构建文件通常由其他工具（如 GN 或 CMake）生成，而不是手动编写。
- **优点**：Ninja 非常快，适用于需要频繁编译的大型项目。

CMake

- **用途**：CMake 是一个跨平台的构建系统生成器，用于生成本地构建系统文件（如 Makefile、Ninja 文件、Visual Studio 项目文件等）。
- **特点**：CMake 使用 `CMakeLists.txt` 文件来描述项目的构建配置，可以生成适用于不同平台和构建工具的配置文件。
- **优点**：CMake 的跨平台支持非常强大，能够生成多种类型的构建系统文件，广泛应用于各种开源项目和企业项目中。

---

[ninja和gn](https://zhuanlan.zhihu.com/p/136954435?utm_oi=1118087249564205056)

```
# 导入其他GN配置文件，这些配置文件可能定义了一些共用的编译选项或者依赖项
import("//build/ohos.gni")
import("./../../../multimedia_camera_framework.gni")

# config块定义了一系列的配置，主要是包括目录的配置。这些配置通过include_dirs指定了编译时需要包含的头文件目录
config("camera_framework_public_config") {
  # 公共配置：指定了公共接口部分的头文件目录
  include_dirs = [...]
}

config("camera_framework_local_config") {
  # 本地配置：指定了服务层相关的头文件目录
  include_dirs = [...]
}

config("camera_framework_drivers_config") {
  # 驱动配置：指定了驱动接口相关的头文件目录
  include_dirs = [...]
}

# 定义了一个名为camera_framework的共享库
ohos_shared_library("camera_framework") {
  # 配置项，如分支保护器返回值、安装启用标志
  branch_protector_ret = "pac_ret"
  install_enable = true
  
  # 源文件：列出了构建这个库所需的源文件
  sources = [...]

  # 编译标志：定义了编译时使用的编译器标志
  cflags = [
    "-fPIC", # 启用位置无关代码
    "-Wall", # 启用所有警告
    "-DSUPPORT_CAMERA_AUTH", # 定义宏，可能用于条件编译
  ]
  # 针对特定CPU架构的条件编译选项
  if (target_cpu == "arm") {
    cflags += [ "-DBINDER_IPC_32BIT" ]
  }

  # 启用了一些安全相关的编译选项，如CFI（控制流完整性）。
  sanitize = {
    cfi = true
    cfi_cross_dso = true
    debug = false
  }

  # 指定了这个库的公共配置依赖，即上面定义的config块
  public_configs = [
    ":camera_framework_public_config",
    ":camera_framework_local_config",
    ":camera_framework_drivers_config",
  ]

  # 包含的头文件目录，具体路径可能通过GN的build配置进行定义
  include_dirs = [...]
  
  # 依赖的外部库：指定了构建这个库需要依赖的其他外部库
  external_deps = [...]

  # 使用sensor的条件编译
  if (use_sensor) {
    external_deps += [ "sensor:sensor_interface_native" ]
    defines += [ "CAMERA_USE_SENSOR" ]
  }

  cflags_cc = cflags # C++编译标志，可能与cflags相同
  innerapi_tags = [ "platformsdk" ] # 标签，可能用于分类或者权限控制
  part_name = "camera_framework" # 部件名称
  subsystem_name = "multimedia" # 子系统名称，这里指多媒体
}
```

---

如果项目由许多源文件组成，并且分布在各个子目录下，如何将它们编译链接到一起呢？

这时候 GNU Make 就闪亮登场了，它能让你在一个脚本里（即所谓的 `Makefile`）定义整个编译流程以及各个目标文件与源文件之间的依赖关系，并且只重新编译你的修改会影响到的部分，从而降低编译的时间。[写Makefile](https://seisman.github.io/how-to-write-makefile/overview.html) 太麻烦了，直接学Cmake吧

CMake 是类似于 GNU make 的跨平台自动软件构建工具，使用 CMakeLists.txt 定义构建规则，相比于 make 它提供了更多的功能，在各种软件构建上广泛使用。[如何学习 CMake?](https://cmake.org/cmake/help/latest/guide/tutorial/index.html)  [视频教程](https://www.bilibili.com/video/BV14h41187FZ)

