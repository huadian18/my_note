#php常用函数
[TOC]
##写入文件
1. 打开资源（文件）fopen($filename,$mode)
2. 写文件fwrite($handle,$str)
3. 关闭文件fclose($handle)
4. 一步写入file_put_contents($filename,$str,$mode) FILE_APPEND LOCK_EX
}

##读文件
1. 读文件fread($handle,字节数)
2. 读一行fgets($handle);
3. 读一个字符fgetc($handle)
4. 读成一个数组中file($filename)
5. 一步读取file_get_contents($filename)

##目录操作
1. 建目录mkdir($dirname)
2. 删除目录rmdir($dirname)
3. 打开目录句柄opendir($dirname)
4. 读取目录条数readdir($handle)
5. 关闭目录资源closedir($handle)
6. 重置目录资源rewinddir($dirname);

##目录和文件操作
1. 检查文件或目录是否存在file_exists($filename)
2. 文件或者目录重命名rename($file)

##文件操作
1. 拷贝文件copy('原文件','目标文件')
2. 删除文件unlink($filename)
3. 获取文件大小filesize($filename)
4. 取得文件的创建时间filectime($filename)
5. 取得文件的访问时间fileatime($filename)
6. 取得文件的修改时间filemtime($filename)

##路径操作
1. 获取路径dirname($path)
2. 获取文件名basename($path)
3. 获取路径信息pathinfo($path)

##数组函数（极其重要）
1. 在数组的开头插入一个元素array_unshift($arr,$v)
2. 在数组的尾部添加数组元素array_push($arr,$v,$v1...)
3. 将数组的第一个元素移出，并返回此元素array_shift($arr)
4. 在数组的尾部删除元素array_pop($arr)
5. 将数组用$separator连接成一个字符串implode($a,$arr)
6. 检测变量是否是数组is_array($arr)
7. 获得数组的键名array_keys($arr)
8. 获得数组的值array_values($arr)
9. 检索$value是否在$arr中，返回布尔值in_array($v,$arr)
10. 检索数组$arr中，是否有$key这个键名array_key_exists($k,$arr)
11. 检索$value是否在$arr中，若存在返回键名Array_search($value, $arr)
12. 将一个数组逆向排序，如果第二个参数为true，则保持键名Array_reverse($arr, true)
13. 交换数组的键和值 Array_flip($arr)
14. 统计数组元素的个数 Count($arr)
15. 统计数组中所有值的出现次数 Array_count_values($arr)
16. 移除数组中的重复值 Array_unique($arr)
17. 值由小到大排序 Sort($arr)
18. 值由大到小排序 Rsort($arr)
19. 键由小到大排序 ksort($arr)
20. 键由大到小排序 krsort($arr)
21. 随机从数组中取得$num个元素 Array_rand($arr, $num)
22. 对数组的所有元素求和Array_sum($arr)
23. 合并数组 array_merge($arr,$arr)

##字符串函数（极其重要）

1. 输出字符串 echo($str) echo
2. 原样输出（区分单引号和双引号） print($str)
3. 输出字符串，结束脚本执行 Die($str):die($str) die;
4. 输出字符串，结束脚本执行 exit($str) exit;
5. 输出格式化字符串 printf($str,$p1,...)
6. 不直接输出格式化的字符串，返回格式化的字符串，保存到变量中 sprintf($str,$p1,...)
7. 打印变量的相关信息 var_dump($p)
8. 字符串转换为小写 strtolower($str)
9. 字符串转换为大写 strtoupper($str)
10. 将字符串的第一个字符转换为大写 ucfirst($str)
11. 将字符串中每个单词转换为大写 ucwords($str)
12. 去除字符串两端的空白字符。 Trim($str,' ,')
13. 去除字符串左边空白字符。 Ltrim($str)
14. 去除字符串右边空白字符。Rtrim($str)
空白字符：""，"\t"，"\n"，"\r"，”\0”
15. 取得字符串长度 strlen($str)
16. 统计包含的字符串个数 substr_count($str,’子串’)
17. 返回字符串$string中由$start开始，长度为$length的子字符串
Substr($string ,$start[,$length])
18. 返回字符串$string中，$search第一次出现到字符串结束的子字符串。
Strstr($string,$search)
19. 查找$search在$str中第一次位的置，从$offset开始。
Strpos($str,$search[,int $offset])
20. 查找$search在$str中最后一次的位置，从$offset开始
Strrpos($str,$search[,int $offset])
21. 替换$str中的全部$search为 $replace。
Str_replace($search,$replace,$str)
22. 重复输出指定的字符串
Str_repeat()
23. 加密字符串
Md5()
24. 字符串翻转
Strrev()
25. 使用一个字符串分割另一个字符串,形成一个数组//把字符串变成数组
Explode(“分隔符”,$str);