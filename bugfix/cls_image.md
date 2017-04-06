### includes/cls_image.php

Strict Standards: Non-static method cls_image::gd_version() should not be called statically

###解决办法：

将 cls_template.php的678行
```
function gd_version()
```

换成：
```
public static function gd_version()
```