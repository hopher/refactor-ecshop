### includes/lib_main.php 
1329 行
```
$ext = end(explode('.', $tmp));
```

换成：
```
$tmp_arr = explode('.', $tmp);
$ext = end($tmp_arr);
```