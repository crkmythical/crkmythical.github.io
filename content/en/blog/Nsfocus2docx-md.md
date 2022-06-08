---
title: Nsfocus2docx.md
encrypt: false
enc_pwd: askding
categories:
  - Kali
  - Exploit
date: 2021-05-23 00:05:22
top:
tags:
layout:
---
编写进度
- [x] 



日常安全扫描场景中，有需要将绿盟的Web漏扫报告(html)转换成docx文档格式的需求，
因此使用python3写了个脚本，使用模块如下
- BeautifulSoup
- docxtpl

## 原始报告

![html](https://raw.githubusercontent.com/askDing/Nsfocus2docx/main/img/nsfocus_html.png)


## 转换成docx报告
![docx](https://raw.githubusercontent.com/askDing/Nsfocus2docx/main/img/nsfocus_docx.png)

## 源码[Nsfocus2docx](https://github.com/askDing/Nsfocus2docx)
```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
# author: Mr.Frame
# Blog: https://askding.github.io/
# Dependence:
#           BeautifulSoup
#           docxtpl
# Bugs:
#  doc.render(data) 渲染数据时，
#  199个漏洞，只能渲染89个，
#  1362个漏洞只能渲染出639个

import sys
import time
from bs4 import BeautifulSoup
from docxtpl import DocxTemplate


# 定义漏洞类
class Vulner:
    def __init__(self, vulnName, ipList, details, threatLevel, solutions):
        self.vulnName = vulnName
        self.ipList = ipList
        self.detail = details
        self.threatLevel = threatLevel
        self.solution = solutions


def banner():
    if len(sys.argv) != 2:
        print(" Usage: python3 main.py  <path/to/index.html>  ")
        sys.exit()


def generate_docx(path):
    data = {"vulners": []}  # 漏洞数据
    soup = BeautifulSoup(open(path), "lxml")
    data['title'] = soup.h1.string  # 文档标题
    generated_dox = data['title'] + time.strftime("%Y-%m-%d", time.localtime()) + ".docx"  # 生成文档名

    vuln_table = soup.find('table', attrs={"id": "vuln_distribution",
                                           "class": "report_table"})

    # 获取漏洞名列表
    vuln_name_list = vuln_table.find_all('span')

    # 获取每个漏洞名字下展开信息
    report_table_list = vuln_table.find_all('table', attrs={"class": "report_table"})

    vuln_List=[]         # 漏洞名字列表

    print("\n梳理漏洞如下: ")
    for vul in vuln_name_list:
        vuln_name = vul.get_text().strip()  # 漏洞名列表
        print(vuln_name)
        vuln_List.append(vuln_name)

    vuln_Hosts=[]        # 风险来源列表
    Detail_List=[]       # 风险分析列表
    Solution_List=[]     # 处置建议列表
    ThreatLevel_List=[]  # 风险级别列表

    for report_table in report_table_list:
        vuln_hosts = report_table.find_all("td")[0].get_text()  # 风险来源
        vuln_hosts = vuln_hosts.replace("&nbsp", "").replace("点击查看详情;", "")

        detail = report_table.find_all("td")[1].get_text()  # 风险分析
        solution = report_table.find_all("td")[2].get_text().strip()  # 处置建议
        threatLevel = report_table.find_all("td")[3].get_text()  # 风险级别

        vuln_Hosts.append(vuln_hosts)
        Detail_List.append(detail)
        Solution_List.append(solution)
        ThreatLevel_List.append(threatLevel)

    for i in range(0,len(vuln_List)):
        print(i)
        data['vulners'].append(
            Vulner(vuln_List[i], vuln_Hosts[i], Detail_List[i], ThreatLevel_List[i], Solution_List[i])
        )
    print("文档原始数据生成中...")
    doc = DocxTemplate("template.docx")
    print("正在渲染文档数据....")
    doc.render(data)
    doc.save(generated_dox)
    print("生成完毕，文档名: {}".format(generated_dox))


if __name__ == '__main__':
    banner()
    generate_docx(sys.argv[1])
```


## 使用
```zsh
python3 Nsfocus2docx  /path/to/NS_report/index.html
```

## 已知Bug

` doc.render(data) `渲染数据时，
- 199个漏洞，只能渲染89个，
- 1362个漏洞只能渲染出639个

欢迎For，PR





