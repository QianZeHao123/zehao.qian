---
title: 'Useful Windows Configuration'
summary: Some settings for Windows 10/11 after I re-installing the system.
tags:
  - Software
  - CS
date: 2023-05-04
# external_link: https://qianzehao123.github.io/posts/2023/07/From-Nand-To-Tetris/
---

## 1 Activate Windows
```powershell
# irm https://massgrave.dev/get | iex
irm https://get.activated.win | iex
```

## 2 Download Office 365
[Download Office Deployment Tool from Official Microsoft Download Center](https://www.microsoft.com/en-us/download/details.aspx?id=49117)

```xml
<!-- config.xml -->
<!-- edit <PathOfDownload> -->
<Configuration>
    <Add SourcePath="<PathOfDownload>" OfficeClientEdition="64">
        <Product ID="O365ProPlusRetail">
            <Language ID="zh-cn" />
            <ExcludeApp ID="Groove" />
            <ExcludeApp ID="Lync" />
            <ExcludeApp ID="OneNote" />
            <ExcludeApp ID="Outlook" />
            <ExcludeApp ID="Publisher" />
            <ExcludeApp ID="Teams" />
            <ExcludeApp ID="Access" />
            <ExcludeApp ID="InfoPath" />
            <ExcludeApp ID="Project" />
            <ExcludeApp ID="SharePointDesigner" />
            <ExcludeApp ID="Visio" />
        </Product>
    </Add>
</Configuration>
```

```powershell
.\setup.exe /download config.xml
.\setup.exe /configure config.xml
```

## 3 Remote Signed for PowerShell

```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned
```

![](./BKP.jpg)

## 4 Necessary Software
{{< icon name="download" pack="fas" >}} {{< staticref "uploads/WSD.zip" "newtab" >}}Download{{< /staticref >}} a fast deployment script.
* [Miniconda](https://docs.anaconda.com/free/miniconda/index.html) for Python Virtual Environment
* [R-language](https://cran.rstudio.com/) R Environment.
* [RStudio](https://posit.co/download/rstudio-desktop/) for writting R code and RMarkdown.
* [Visual Studio Code](https://code.visualstudio.com/) for almost any language in the world!
* [Bandizip](https://www.bandisoft.com/bandizip/) Compression software for professionals.
* [Notion](https://www.notion.so/desktop) Download the Notion desktop app for a faster experience.
* [Obsidian](https://obsidian.md/) another choice of Notion.
* [Git](https://git-scm.com/download/win) for version control.
* [Go-lang](https://golang.google.cn/dl/) is the Go language SDK download.
* [Typst](https://github.com/typst/typst/releases/) is the best alternative to $\LaTeX$.
* [Oracle Java](https://www.oracle.com/java/technologies/downloads/) Official JDK.
* [Blender](https://www.blender.org/download/) for 3D making.

## 5 Download Scoop
```powershell
Invoke-Expression (New-Object System.Net.WebClient).DownloadString('https://get.scoop.sh')
```
