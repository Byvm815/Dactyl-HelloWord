230316
使用稚辉君同款的编译器：

下载arm-none-eabi-gcc，更改Clion的c和c++编译器，cmakelists.text使用默认配置（仅更改了版本号），报错如下：
```
/Applications/CLion.app/Contents/bin/cmake/mac/bin/cmake -DCMAKE_BUILD_TYPE=Debug -DCMAKE_C_COMPILER=/Applications/ARM/bin/arm-none-eabi-gcc -DCMAKE_CXX_COMPILER=/Applications/ARM/bin/arm-none-eabi-g++ -DCMAKE_DEPENDS_USE_COMPILER=FALSE -G "CodeBlocks - Unix Makefiles" /Volumes/SanDisk/Byme/Dactyl-HelloWord/Dactyl-HelloWord/Firmware/Byvm/HelloWord-Keyboard-fw
-- The C compiler identification is unknown
-- The CXX compiler identification is unknown
-- The ASM compiler identification is unknown
-- Found assembler: arm-none-eabi-gcc
CMake Error at CMakeLists.txt:19 (project):
  The CMAKE_C_COMPILER:

    arm-none-eabi-gcc

  is not a full path and was not found in the PATH.

  Tell CMake where to find the compiler by setting either the environment
  variable "CC" or the CMake cache entry CMAKE_C_COMPILER to the full path to
  the compiler, or to the compiler name if it is in the PATH.


CMake Error at CMakeLists.txt:19 (project):
  The CMAKE_CXX_COMPILER:

    arm-none-eabi-g++

  is not a full path and was not found in the PATH.

  Tell CMake where to find the compiler by setting either the environment
  variable "CXX" or the CMake cache entry CMAKE_CXX_COMPILER to the full path
  to the compiler, or to the compiler name if it is in the PATH.


CMake Error at CMakeLists.txt:19 (project):
  The CMAKE_ASM_COMPILER:

    arm-none-eabi-gcc

  is not a full path and was not found in the PATH.

  Tell CMake where to find the compiler by setting either the environment
  variable "ASM" or the CMake cache entry CMAKE_ASM_COMPILER to the full path
  to the compiler, or to the compiler name if it is in the PATH.


-- Warning: Did not find file Compiler/-ASM
-- Configuring incomplete, errors occurred!
See also "/Volumes/SanDisk/Byme/Dactyl-HelloWord/Dactyl-HelloWord/Firmware/Byvm/HelloWord-Keyboard-fw/cmake-build-debug/CMakeFiles/CMakeOutput.log".
See also "/Volumes/SanDisk/Byme/Dactyl-HelloWord/Dactyl-HelloWord/Firmware/Byvm/HelloWord-Keyboard-fw/cmake-build-debug/CMakeFiles/CMakeError.log".

[Finished]

```

将arm-none-eabi-gcc路径添加到/etc/paths，注意要重启后才能生效。之前用Path命令，无法在Clion中找到。现在可以完成编译了。

发现在VSCode打开，使用GCC编译器也可以正常编译。

总结：
过程中安装的软件：
1. CMake
2. OpenOCD
3. GCC
4. arm-none-eabi-gcc
5. VScode (闭源软件，基于开源的Code OSS)

VSCode插件：
1. C/C++（代码公开，但非自由软件）（可使用Clangd替代，但代码有少量错误提示）
2. CMake
3. CMake Tools

主要参考了稚晖君的这篇[文章](https://zhuanlan.zhihu.com/p/145801160)。

230317
微软的C/C++插件没有跳转提示，所以才没有错误？

通过控制变量，在相同的配置文件、相同的编译工具下编译，通过对比Clion和VSCode所编译的二进制，可知二进制代码完全相同，并未因编辑器不同而导致编译差异。

230318
```
[variant] Loaded new set of variants
[kit] Successfully loaded 2 kits from /Users/byvm/.local/share/CMakeTools/cmake-tools-kits.json
[proc] Executing command: /usr/local/bin/cmake --version
[proc] Executing command: /usr/local/bin/gcc-12 -v
[proc] The command: ninja --version failed with error: Error: spawn ninja ENOENT
[proc] The command: ninja-build --version failed with error: Error: spawn ninja-build ENOENT
[main] Configuring project: Byvm 
[proc] Executing command: /usr/local/bin/cmake --no-warn-unused-cli -DCMAKE_EXPORT_COMPILE_COMMANDS:BOOL=TRUE -DCMAKE_BUILD_TYPE:STRING=Release -DCMAKE_C_COMPILER:FILEPATH=/usr/local/bin/gcc-12 -DCMAKE_CXX_COMPILER:FILEPATH=/usr/local/bin/g++-12 -S/Volumes/SanDisk/Byme/Dactyl-HelloWord/Dactyl-HelloWord/Firmware/Byvm/HelloWord-Keyboard-fw -B/Volumes/SanDisk/Byme/Dactyl-HelloWord/Dactyl-HelloWord/Firmware/Byvm/build -G "Unix Makefiles"
[cmake] Not searching for unused variables given on the command line.
[cmake] -- Maximum optimization for speed
[cmake] -- Configuring done (0.0s)
[cmake] You have changed variables that require your cache to be deleted.
[cmake] Configure will be re-run and you may have to reset some variables.
[cmake] The following variables have changed:
[cmake] CMAKE_C_COMPILER= /usr/local/bin/gcc-12
[cmake] CMAKE_CXX_COMPILER= /usr/local/bin/g++-12
[cmake] 
[cmake] -- The C compiler identification is GNU 10.3.1
[cmake] -- The CXX compiler identification is GNU 10.3.1
[cmake] -- The ASM compiler identification is GNU
[cmake] -- Found assembler: /Applications/ARM/bin/arm-none-eabi-gcc
[cmake] -- Detecting C compiler ABI info
[cmake] -- Detecting C compiler ABI info - done
[cmake] -- Check for working C compiler: /Applications/ARM/bin/arm-none-eabi-gcc - skipped
[cmake] -- Detecting C compile features
[cmake] -- Detecting C compile features - done
[cmake] -- Detecting CXX compiler ABI info
[cmake] -- Detecting CXX compiler ABI info - done
[cmake] -- Check for working CXX compiler: /Applications/ARM/bin/arm-none-eabi-g++ - skipped
[cmake] -- Detecting CXX compile features
[cmake] -- Detecting CXX compile features - done
[cmake] -- Minimal optimization, debug info included
[cmake] -- Configuring done (1.6s)
[cmake] -- Generating done (0.8s)
[cmake] -- Build files have been written to: /Volumes/SanDisk/Byme/Dactyl-HelloWord/Dactyl-HelloWord/Firmware/Byvm/build
```

虽然可以编译了，但是注意到提示中有一个error,VSCode的提示没有颜色区分真令人头痛。不过不影响，因为稚晖君的开发环境也没有这个ninja。

230321
使用C/C++插件需要在Settings.json里配置这个选项为默认（这个参考https://www.bilibili.com/video/BV1gD4y1y7um）
```
"C_Cpp.intelliSenseEngine": "default"
```

然后要更改C/C++插件使用的编译器，在 Byvm/.vscode/c_cpp_properties.json
```
"compilerPath": "/Applications/ARM/bin/arm-none-eabi-gcc"
```

用DACTYL-HWLLOWORD做工作根目录打开main.c会出现以下问题，但以Byvm做根目录则不会。
```
没有找到库：stdint-gcc.h
```

当然，可以手动添加库路径
```
/Applications/ARM/lib/gcc/arm-none-eabi/10.3.1/include/stdint-gcc.h
```
230327
经过了各种尝试：
1. 设置settings.json中的clangd插件的库文件路径
2. 通过配置clangd插件的PATH路径
3. 在cmakelists中的include_directories()添加arm-none-eabi-gcc的库文件路径

以上的方法都无法解决找不到库文件的问题。

但在手动拷贝缺失的库文件到工作目录，并在Cmakelists.txt中引入，库文件缺失的问题就没有了。但在引用arm-none-eabi-gcc默认的安装位置中的库文件就会报错，令人不解，大概率是我的语法和引用函数不对。

目前main.c有三个错误提示：
1. 在第一行注释的错误：
```
Unknown argument: '-mthumb-interwork'clang(drv_unknown_argument)
```

2. main.h的错误：
```
In included file: "Compiler generates FPU instructions for a device without an FPU (check __FPU_PRESENT)"clang(pp_hash_error)
```

3. usb_device.h的错误
```
In included file: 'stdio.h' file not foundclang(pp_file_not_found)
usbd_conf.h(31, 10): Error occurred here
```

230329
发现Clion默认配置的Cmake会对不同的编译类型，单独生成一个Build文件夹。如cmake-build-debug、cmake-build-release。确实是设计地好啊。

我又忘了看Clangd输出报错，光上网查错误提示是一点用也没有，就像我开始安装arm-none-eabi-gcc时，无法查找到一样。Clangd有一个输出面板，只是要把下拉箭头点开才看得到。

其报错如下：
```
I[17:43:11.016] --> reply:textDocument/documentLink(1) 388 ms, error: Task was cancelled.

[Error - 5:43:11 PM] Request textDocument/documentLink failed.
```

经过查找，外国网友说以上的问题提示并不是Clangd的问题，而是由VSCode客户端打印的。

注意到，Clion生成的build文件夹并没有compile_commands.json文件，本来还想这，既然Clion也用Clangd，应该也有compile_commands.json可以用于对比。

打开这个选项，Clion就有compile_commands.json文件了
```
set(CMAKE_EXPORT_COMPILE_COMMANDS True)
```

对比两个编辑器所生成的文件后发现，除了工作根目录不同导致的路径差异外，两者没有任何区别。

main.h的错误提示，不知道怎么就突然不报错了。

230330
在使用默认配置时，发现main.h的错误提示又回来了。而C/Clangd配置中的Clangd只是改为brew下载的clangd。对比后发现，更改为非apple的clangd确实没有了main.h的错误提示。

根据B站的视频（之前还看过一眼）（https://www.bilibili.com/video/BV1RM411U7hr/），--query-driver选项是要填编译器路径的，而网上找的教程都是说填Clangd路径。现在stdio的问题解决了。

从cpptools的配置来看，确实是要填编译器路径的，但是我就是找不到Clangd的选项在哪。而网上的教程都是配置Clangd路径。

将cmakelists中的
```
-mthumb-interwork
```
改为
```
-mno-thumb interworking
```
不知道该选项的错误提示没有了，但是编译的时候有错误提示，提示未知命令。

新的错误提示：
```
CPU 'cortex-m3' does not support 'ARM' execution modeclang(cpu_unsupported_isa)
```

发现在Clion中，如果使用上面有错误的编译选项，也会提示上面CPU的相关错误提示。但是默认的编译选项在Clion是正常的。

230401
无论是VSCode还是VSCodium，在安装CodeLLDB时，会下载一个额外的扩展，但无论我是等待插件下载完成，还是手动安装VSIX，在安装完成后CodeLLDB插件就会在我的扩展中消失。