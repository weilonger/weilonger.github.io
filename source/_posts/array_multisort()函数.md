---
title: array_multisort（)函数
date: 2017-05-15
tags: [php,数组,排序]
categories: php
---

data 数组中的每个单元表示一个表中的一行。这是典型的数据库记录的数据集合。
## 数据
volume |edition 
-- | --
67  |  2
86  |  1
85  |  6
98  |  2
86  |  6
67  |  7

数据全都存放在名为 data 的数组中。这通常是通过循环从数据库取得的结果，例如 mysql_fetch_assoc()。
```
<?php
$data[] = array('volume' => 67, 'edition' => 2);
$data[] = array('volume' => 86, 'edition' => 1);
$data[] = array('volume' => 85, 'edition' => 6);
$data[] = array('volume' => 98, 'edition' => 2);
$data[] = array('volume' => 86, 'edition' => 6);
$data[] = array('volume' => 67, 'edition' => 7);
?>
```
## 排序
现在有了包含有行的数组，但是 array_multisort() 需要一个包含列的数组，因此用以下代码来取得列，然后排序。

```
<?php
// 取得列的列表
foreach ($data as $key => $row) {
    $volume[$key]  = $row['volume'];
    $edition[$key] = $row['edition'];
}

// 将数据根据 volume 降序排列，根据 edition 升序排列
// 把 $data 作为最后一个参数，以通用键排序
array_multisort($volume, SORT_DESC, $edition, SORT_ASC, $data);
?>
```

## 展示
volume | edition
-- | --
98 |       2
86 |       1
86 |       6
85 |       6
67 |       2
67 |       7


## 报错
Warning: array_multisort(): Argument #5 is expected to be an array or a sort flag