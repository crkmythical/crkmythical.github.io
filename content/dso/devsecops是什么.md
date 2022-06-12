---
title: "Devsecops是什么"
author: "Hugo Authors"
description: ""
date: 2022-06-08T22:39:30+08:00
draft: false

weight: 0
image: "" # relative path to the static folder. The image is in the root/static/images folder.
enableToc: true
tocLevels: ["h2", "h3", "h4"]
tags:
-
#collapsible: true
---



# Table of Contents

1.  [代码审计常规手法](#orgb5a227d)
    1.  [Front](#org5e57c66)
    2.  [Back](#org634603f)
    3.  [审计流程四步骤](#org60c84f5)
        1.  [Front](#org43a5a2e)
        2.  [Back](#orgceb20ad)



<a id="orgb5a227d"></a>

# 代码审计常规手法


<a id="org5e57c66"></a>

## Front

代码审计常规手法


<a id="org634603f"></a>

## Back

-   **通读源码:** 从入口函数开始读
-   **关键函数回溯:** 先定位敏感函数及参数,同步回溯参数赋值过程,判断是否可控以及是否经过过滤等
-   **追踪功能点:** 根据经验判断可能存在问题的路由或功能点,对该功能点进行通读


<a id="org60c84f5"></a>

## 审计流程四步骤


<a id="org43a5a2e"></a>

### Front

审计流程四步骤


<a id="orgceb20ad"></a>

### Back

-   **1.获取源码,搭建环境:** Github、GitLab、CSDN、Gitee等
-   **2.半自动化代码审计:** SAST工具扫描,功能点审计、关键函数溯源、通读源码
-   **3.POC编写:** 根据审计结果以及触发方式编写POC,通过POC确定问题及造成影响
-   **4.报告编写:** 根据审计出的安全问题和POC脚本的验证结果,编写整体的代码审计报告


