1. $layout = 'main';当前模块  $layout = '/main'; views下的layouts  其他书写方式
2. http://www.phpernote.com/php-template/1073.html
3. PHP数据类型有三种转换方式：
（1）在要转换的变量之前加上用括号括起来的目标类型，例如：
(int)  (bool)  (float)  (string)  (array) (object) 下面通过实例说明：
查看代码
打印
    <?php
    $num1=3.14;
    $num2=(int)$num1; //强制转换为int类型
    var_dump($num1); //输出float(3.14)
    var_dump($num2); //输出int(3)
（2）使用3个具体类型的转换函数，intval()、floatval()、strval() ，实例如下：
查看代码
打印
    <?php
    $str="123.9abc";
    $int=intval($str); //转换后数值：123
    $float=floatval($str); //转换后数值：123.9
    $str=strval($float); //转换后字符串："123.9"
（3）使用通用类型转换函数settype(mixed var,string type) ，具体实例如下：
查看代码
打印
    <?php
    $num4=12.8;
    $flg=settype($num4,"int");
    var_dump($flg); //输出bool(true)
    var_dump($num4); //输出int(12)
4. 正则php js
5. $array=array_filter($array,create_function('$v','return !empty($v);'));
    print_r($array);   删除数组空元素
6. 使用 Glob() 查找文件
7. 当前内存使用情况，memory_get_usage() 函数，内存的峰值， memory_get_peak_usage() 函数
8. uniqid(prefix,more_entropy)  基于以微秒计的当前时间，生成一个唯一的 ID
9. 字符串压缩  gzcompress() 和 gzuncompress()
10. http://www.phpernote.com/php-function/374.html
11. js eval()
12. http://www.yiichina.com/question/1528
13. php5.4 内置服务器