Ubuntu 环境下eclipse中项目中文乱码问题解决
====

**Author**: Neal Zhang

**Email**: Loong@pplns.com

此文档用于提供Ubuntu环境下导致eclipse中项目中文乱码问题的解决方案

====
目录
------
*   [前言] (#前言)
*   [设置Ubuntu系统编码] (#设置Ubuntu系统编码)
*   [设置Eclipse编码] (#设置Eclipse编码)

前言
------
    eclipse在Ubuntu系统中默认编码是utf-8，在windows系统中默认编码是GBK，所以从windows系统导入到Ubuntu系统中的eclipse项目就会出现乱码问题。

设置Ubuntu系统编码
------
    vi /var/lib/locales/supported.d/local

    在文件中添加一下内容：

    zh_CN.GBK GBK

    zh_CN.GB2312 GB2312

    执行以下语句：

    dpkg-reconfigure --force locales或locale-gen

    然后在输出的结果中会出现

    zh_CN.GB2312 done

    zh_CN.GBK done

设置Eclipse编码
------
    选择eclipse菜单栏中的Windows->Preferences->General->Text file encoding->Other GBK，如果没有GBK的选项，直接输入GBK三个字母，Apply，GBK编码的中文，已经不是乱码了。



