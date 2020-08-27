# afl-cov: afl-fuzz代码覆盖率可视化工具
afl-cov是afl-fuzz的代码覆盖度测试插件，能够获取被测试程序的源码覆盖率，监测源码中被执行过的代码路径以及具体的函数名称，帮助漏洞挖掘人员分析样本在测试程序中的执行情况，辅助分析人员有效地改进样本集，以达到覆盖更多执行路径的目的。

## Overiew
afl-cov核心是调用lcov和gcov指令，它俩是GCC的代码覆盖率测试工具，其中gcov测试源代码，lcov测试的是.gcda文件，并将生成的结果交给genhtml程序，使其生成相应的web目录，包含源代码执行情况的html文件。
简言之，工作流如下：
> * gcc -fprofile-arcs -ftest-coverage 
> * afl-fuzz运行somebin，跑样本集
> * afl-cov调用gcov执行afl-fuzz-output目录下的queue子目录中的样本集
> * afl-cov调用lcov捕获somebin执行后的代码覆盖信息，生成报告

## Usage
./afl-cov -d /path/to/afl-fuzz-output/ --live --coverage-cmd \

"LD_LIBRARY_PATH=./lib/.libs ./bin/.libs/somebin -i AFL_FILE -o out.bt" \

--code-dir code-dir  --lcov-web-all

其中 --coverage-cmd是需要被执行的内部命令，它的参数中AFL_FILE会被替换成afl-fuzz-output下的queue子目录中的样本集，此处只具有指代意义
另外 --lcov-web-all是使用genhtml产生web目录，里面每个样本被执行后都会产生相应的web目录，其中index.html文件记录代码覆盖情况。
还有--code-dir 后跟被测试对象的源码根目录

比如说，如果afl正在fuzz如下指令的程序：
```sh
afl-fuzz -i input -o output xxx -a x86 -d path -f @@
```
那么使用afl-cov的命令为：
```sh
afl-cov -d /path/to-afl-fuzz-output/ --live --coverage-cmd \
"xxx -i AFL_FILE -a x86 -d path -f" \
--code-dir code-dir  --lcov-web-all
```

## [linkage][1]
[1]: https://github.com/mrash/afl-cov
