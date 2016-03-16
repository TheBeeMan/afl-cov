# afl-cov: afl-fuzz代码覆盖度测试工具
afl-cov是afl-fuzz的代码覆盖度测试插件，能够获取被测试程序的源码覆盖率，监测源码中被执行过的代码路径以及具体的函数名称，帮助漏洞挖掘人员分析样本在测试程序中的执行情况，辅助分析人员有效地改进样本集，以达到覆盖更多执行路径的目的。
##原理
afl-cov本质是python脚本，核心是调用lcov和gcov指令。它们是系统级的代码覆盖率测试工具，其中gcov测试源代码，lcov测试的是.gcda文件，并将生成的结果交给genhtml程序，使其生成相应的web目录，包含源代码执行情况的html文件。
简言之，工作流如下：
> * gcc -fprofile-arcs -ftest-coverage 
> * afl-fuzz运行被somebin，跑样本集
> * afl-cov调用copybin执行afl-fuzz-output目录下的queue子目录中的样本集
> * afl-cov调用lcov捕获copybin执行后的代码覆盖信息，生成报告

## 用法
./afl-cov -d /path/to/afl-fuzz-output/ --live --coverage-cmd \
"LD_LIBRARY_PATH=./lib/.libs ./bin/.libs/somebin -i AFL_FILE -o out.bt" \
--code-dir code-dir
