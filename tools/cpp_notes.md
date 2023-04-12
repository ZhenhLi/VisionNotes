# Topic: notes for cpp

## list

## 

## 读书笔记
  * 

## 调试经验
* 知乎问题：大型C++项目如何在linux下调试?
  * 要点记录 CrackingOysters
    * 命令行 gdb or lldb. 该有的都有，功能很强大.
      * 常见的断点: break point, function point, watch point等. ??
        * break point
        * function point
        * watch point ?
      * 设置某个断点hit时做各种事情: 如打印调用栈、提取信息等.
      * 结合python, 自动定位到死锁、自动去除无关线程调用栈，遍历整个heap判断是否为memory corruption，以及遍历整个heap获取程序的内存使用情况(发现内存异常)等等.
      * 将debug步骤做成python脚本，只需敲一行命令，自动打印.
      * 逆向调试.
      * Google sanitizer找内存错误和多线程问题.

  * ?
    * 逻辑错误用log
    * 看调用栈用gdb
    * 单元测试用gtest
    * 性能热点用gprof
    * 内存错误用valgrind
    * 