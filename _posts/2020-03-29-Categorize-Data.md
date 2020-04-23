---
layout:     post
title:      "How to Categorize Data"
subtitle:   "数据科学基础课笔记（一）"
date:       2020-03-29 20:00:00
author:     "AH"
header-img: "img/post-bg-2015.jpg"
tags:
    - Data Science
---



# 数据的层次

<table>
	<tr>
		<td colspan="2"><b>数据的层次</b></td>
		<td><b>描述</b></td>
		<td><b>支持的数学运算</b></td>
        <td><b>例子</b></td>
	</tr>
	<tr>
		<td rowspan="2">Qualitative</td>
		<td>Nominal Level</td>
        <td>名词形式的数据</td>
        <td>集合操作（交集、并集）</td>
        <td>动物的名称</td>
	</tr>
	<tr>
		<td>Ordinal Level</td>
        <td>能够进行排序的数据</td>
        <td>集合操作 <br> 比较</td>
        <td>成绩</td>
	</tr>
	<tr>
		<td rowspan="2">Quantitative</td>
		<td>Interval Level</td>
        <td>减法有意义的数据</td>
        <td>集合操作 <br> 比较 <br> （加）减法</td>
        <td>摄氏度</td>
	</tr>
	<tr>
		<td>Ratio Level</td>
        <td>除法有意义的数据</td>
        <td>集合操作 <br> 比较 <br> （加）减法 <br> （乘）除法</td>
        <td>开尔文（绝对温标）</td>
	</tr>
</table>



# 细分Quantitative Data

* 离散/连续：

  * 离散：数数
  * 连续：身高/体重

* 结构化/非结构化

  * 结构化：二元组/三元组/list/树/图
  * 非结构化：多媒体数据（文本/图片/视频）
  * 从非结构化->结构化：提取特征
