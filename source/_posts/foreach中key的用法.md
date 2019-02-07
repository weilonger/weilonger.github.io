---
title: php 循环
date: 2017-05-14
tags: [php循环]
categories: php
---
## 原生写法

```
foreach($arr as $k=>$v){
if($k%2 == 0){
echo $arr[$k] - $arr[$k+1];
}
}
```
## tp标签
```
<foreach name="$arr" item="v" key="k">
<if condition="$k%2 eq 0">
<php>echo $arr[$k] - $arr[$k+1];</php>
</if>
</foreach>
```

## volist
在使用一个火车票接口的时候，碰到如下问题
错误写法
```
    共有{$trainno['station_list']|count}个站点
    <table class='table table-striped table-bordered table-hover'>
        <tr>
            <th colspan="5">火车路过站点信息</th>
        </tr>
        <tr>
            <th>站点id</th>
            <th>站点名称</th>
            <th>到达时间</th>
            <th>离开时间</th>
            <th>停留时间</th>
        </tr>   
        
        <volist name="trainno" id="info">
            <tr>
                <td style="text-align: center">{$info['station_list']['train_id']}</td>
                <td style="text-align: center">{$info['station_list']['station_name']}</td>
                <td style="text-align: center">{$info['station_list']['arrive_time']}</td>
                <td style="text-align: center">{$info['station_list']['leave_time']}</td>
                <td style="text-align: center">{$info['station_list']['stay']}</td>  
            </tr>
        </volist>
    </table>
```
试了一遍又一遍始终没能输出来
然后，把我最喜欢的手册拿了出来，发现礼拜内有一段代码,是在标签嵌套里边的
```
    <volist name="list" id="vo">    
        <volist name="vo['sub']" id="sub">        
            {$sub.name}    
        </volist>
    </volist>
```
然后报着侥幸的心理决定试一试，最后果然成功了
正确写法
```
    共有{$trainno['station_list']|count}个站点
    <table class='table table-striped table-bordered table-hover'>
        <tr>
            <th colspan="5">火车路过站点信息</th>
        </tr>
        <tr>
            <th>站点id</th>
            <th>站点名称</th>
            <th>到达时间</th>
            <th>离开时间</th>
            <th>停留时间</th>
        </tr>   
        
        <volist name="trainno['station_list']" id="info">
            <tr>
                <td style="text-align: center">{$info['train_id']}</td>
                <td style="text-align: center">{$info['station_name']}</td>
                <td style="text-align: center">{$info['arrive_time']}</td>
                <td style="text-align: center">{$info['leave_time']}</td>
                <td style="text-align: center">{$info['stay']}</td>  
            </tr>
        </volist>
    </table>
```

虽然说这个问题不是很大，但我觉得还是要写一写。