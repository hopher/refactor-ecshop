### includes/cls_template.php

如何解决DEPRECATED: PREG_REPLACE()报错

类似这样的报错：

Deprecated: preg_replace(): The /e modifier is deprecated, use preg_replace_callback instead in D:\wyh\ecshop\includes\cls_template.php on line 300

###错误原因：
preg_replace() 函数中用到的修饰符 /e 在 PHP5.5.x 中已经被弃用了。
如果你的PHP版本恰好是PHP5.5.X，那你的ECSHOP肯定就会报类似这样的错误。

###解决办法：

一、将 cls_template.php的300行
```
return preg_replace("/{([^\}\{\n]*)}/e", "\$this->select('\\1');", $source);
```

换成：
```
return preg_replace_callback("/{([^\}\{\n]*)}/", function($r) { return $this->select($r[1]); }, $source);
```

二、将cls_template.php的493行

```
$out = "<?php \n" . '$k = ' . preg_replace("/(\'\\$[^,]+)/e" , "stripslashes(trim('\\1','\''));", var_export($t, true)) . ";\n";
```

换成：

```
$out = <?php \n" . '$k = ' . preg_replace_callback("/(\'\\$[^,]+)/" , function($r) {return stripslashes(trim($r[1],'\''));}, var_export($t, true)) . ";\n";
```

三、将cls_template.php的552行

```
$val = preg_replace("/\[([^\[\]]*)\]/eis", "'.'.str_replace('$','\$','\\1')", $val);
```

换成：

```
$val = preg_replace_callback("/\[([^\[\]]*)\]/is", function($r) {return '.'.str_replace('$','$',$r[1]);}, $val);
```

四、将cls_template.php的1069行

```
$pattern = '/<!--\s#BeginLibraryItem\s\"\/(.*?)\"\s-->.*?<!--\s#EndLibraryItem\s-->/se';
$replacement = "'{include file='.strtolower('\\1'). '}'";
$source = preg_replace($pattern, $replacement, $source);
```

换成：

```
$pattern = '/<!--\s#BeginLibraryItem\s\"\/(.*?)\"\s-->.*?<!--\s#EndLibraryItem\s-->/s';
$source = preg_replace_callback($pattern, function($r){return '{include file='.strtolower($r[1]). '}';}, $source);
```