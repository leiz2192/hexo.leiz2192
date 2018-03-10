---
title: C++命令行参数解析
---
# 1 Linux

在Linux环境可以使用getopt解析命令行参数.

## 1.1 main

main()函数声明:

```c++
int main(int argc, char *argv[]);
```

从命令行传递给程序main()函数的包含两个部分的内容, argc 参数包含参数的计数值，而 argv 包含指向这些参数的指针数组.

## 1.2 getopt

函数getopt位于unistd.h系统头文件中. Windows 没有这个头文件, 所有建议在Linux中使用.

getopt原型

```c++
int getopt( int argc, char *const argv[], const char *optstring );
```

给定了命令参数的数量 (argc)、指向这些参数的数组 (argv) 和选项字符串 (optstring) 后，getopt() 将返回第一个选项，并设置一些全局变量。使用相同的参数再次调用该函数时，它将返回下一个选项，并设置相应的全局变量。如果不再有识别到的选项，将返回 -1，此任务就完成了。

getopt() 所设置的全局变量包括：

- optarg -- 指向当前选项参数（如果有）的指针。
- optind -- 再次调用 getopt() 时的下一个 argv 指针的索引。
- optopt -- 最后一个已知选项。

对于每个选项，选项字符串 (optstring) 中都包含一个对应的字符。具有参数的选项（如示例中的 -l 和 -o 选项）后面跟有一个 : 字符。示例所使用的 optstring 为 Il:o:vh?（前面提到，还要支持最后两个用于打印程序的使用方法消息的选项）。

可以重复调用 getopt()，直到其返回 -1 为止；任何剩下的命令行参数通常视为文件名或程序相应的其他内容。

## 1.3 示例

```c++
#include <unistd.h>
#include <string>
#include <stdio.h>
 
int main(int argc, char* argv[])
{
  std::string file_name;
  int c = 0;
  bool s_internal = false;
  bool c_internal = false;
  while ((c = getopt(argc, argv, "scf:h")) != -1) {
    switch (c) {
      case 'c':
        c_internal = true;
        break;
      case 's':
        s_internal = true;
        break;
      case 'f':
        file_name = optarg;
        break;
      case 'h':
        break;
      case ':':   /* missing option argument */
        fprintf(stderr, "%s: option `-%c' requires an argument\n", argv[0], optopt);
        throw "requires an argument";
      case '?':
      default:    /* invalid option */
        fprintf(stderr, "%s: option `-%c' is invalid: ignored\n", argv[0], optopt);
        throw "option invalid";
    }
  }
   
  return 0;
}
```
# 2 Windows

在Windows可以使用的Boost.Program_options库解析命令行参数.

## 2.1 示例

```c++
#include <iostream>
#include <boost/program_options.hpp>
 
int main(int argc, char *argv[])
{
    try {
        namespace po = boost::program_options;
        po::options_description desc("Allowed options");
        desc.add_options()
            ("xml_to_xls,x", po::value<std::string>(), "from_xml_to_xls")
            ("xls_to_xml,s", po::value<std::string>(), "from_xls_to_xml")
            ("out,o", po::value<std::string>(), "assign output file name");
 
        po::variables_map vm;
        po::store(parse_command_line(argc, argv, desc), vm);
        po::notify(vm);
 
        std::string out_file;
        if (vm.count("out")) {
            out_file = vm["out"].as<std::string>();
        }
 
        if (vm.count("xml_to_xls")) {
            from_xml_to_xls(vm["xml_to_xls"].as<std::string>(), out_file);
        }
        else if (vm.count("xls_to_xml")) {
            from_xls_to_xml(vm["xls_to_xml"].as<std::string>(), out_file);
        }
        else {
            std::cout << desc;
        }
    } catch (ModalDefToolException& e) {
        std::cout << e.what()<<  "\n";
    } catch (std::logic_error& e) {
        std::cout << e.what()<<  "\n";
    } catch (...) {
        std::cout << "Unkown exception\n";
    }
 
    return 0;
}
```

## 2.2 组件

program_options的使用主要通过下面三个组件完成：

| 组件名                          | 作用                         |
| ------------------------------- | ---------------------------- |
| options_description(选项描述器) | 描述当前的程序定义了哪些选项 |
| parse_command_line(选项分析器)  | 解析由命令行输入的参数       |
| variables_map(选项存储器)       | 容器,用于存储解析后的选项    |

## 2.3 流程

```flow
st=>start: Start
ed=>end: End
op1=>operation: 构造option_description对象和variables_map对象
op2=>operation: add_options(): 向option_description对象添加选项
op3=>operation: parse_command_line(): 将命令行输入的参数解析出来
op4=>operation: store(): 将解析出的选项存储至variables_map中
op5=>operation: notify(): 通知variables_map去更新所有的外部变量
op6=>operation: count(): 检测某个选项是否被输入
op7=>operation: operator[]: 取出选项的值
st->op1->op2->op3->op4->op5->op6->op7->ed

```

## 2.4 下载

Boost已经提供了编译好的Visual Studio版本, 可以直接[下载](https://dl.bintray.com/boostorg/release/1.65.1/binaries/)使用.