## C++

### compile

C++ compile

```shell
g++ -Wall -Werror -pedantic -g --std=c++17 -Wno-sign-compare -Wno-comment main.cpp -o main.exe
```

这里包括了: 

| 选项                | 作用                                        |
| ------------------- | ------------------------------------------- |
| `-Wall`             | 打开几乎所有常用的警告信息                  |
| `-Werror`           | 将所有警告视为错误，防止警告被忽略          |
| `-pedantic`         | 强制标准 C++ 规范，报告任何不符合标准的行为 |
| `-g`                | 生成调试信息，用于 gdb 等调试器             |
| `--std=c++17`       | 明确使用 C++17 标准（默认可能是更旧的）     |
| `-Wno-sign-compare` | 关闭有关有符号/无符号类型比较的警告         |
| `-Wno-comment`      | 关闭注释中格式错误（如嵌套 `/* */`）的警告  |

简洁版:

```shell
g++ *.cpp -o main.exe 
```

### 列举include路径

```shell
g++ -v -E -x c++ -
```



### memory leak check

```shell
// memory leak checking
MallocStackLogging=1 leaks -quiet -atExit -- ./main.exe
```





## 文件处理

解压

```shell
tar -zxvf <filename.tar.gz> -C <directory>
```



## VSCode 的问题





// 解决 VSCode include path 的问题
添加setting.json 文件, 并添加以下内容:

```json
{
    "C_Cpp.default.compilerPath": "/opt/homebrew/bin/g++-13"
}
或者 cmd+shift+p, select intellisence configuration, 使用 gcc/g++-13

// in c_cpp_properties_json (Mac: cmd+shift+p打开) (win: ctrl+shift+p打开)
"includePath": [
                "${workspaceFolder}/**",
                "/Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX.sdk/usr/include/c++/v1", 
                "/Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/lib/clang/14.0.0/include",
                "/Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX.sdk/usr/include",
                "/Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/include",
                "/Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX.sdk/System/Library/Frameworks"
            ],
```



