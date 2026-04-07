---
title: Go语言中Xorm Reverse的使用
date: 2025-02-15 23:47:58
tags:
- Go语言
- Xorm
categories: 后端开发
cover: https://pub-2f039bcc31c44eb8a3d5406db7997c4c.r2.dev/wp7041136-golang-wallpapers.png
---
## 开端
最近在学习GO语言，在跟着教程做项目的时候，教程里直接略去了Xorm的具体讲解，并且由于视频的年头比较长，现在Xorm已经迁移很多次，故此更新一下博客记录一下花了一下午才处理好的Xorm Reverse的使用方式。

## Xorm Reverse是干嘛的
Xorm Reverse的作用其实就是从数据库直接读取schema生成对应的结构体文件，这样我们就不用辛辛苦苦手写结构体代码了。

## 为啥不去看文档
因为文档没写

作为长久以来一直将Java做为主语言的码农，我已经习惯了即使是最简单的问题在Stack Overflow或者各种其他网站上都存在着大量回答的状态。各种框架的官方文档永远细致清晰更新很快。Xorm的官网只能说一言难尽，聊胜于无。而且由于Go语言版本的变化，还有Xorm官方代码库迁来迁去导致即使能找到一些古早的博客，基本现在直接粘贴过来也是没法儿用的。要想使用支持最新版本的，还要去看官网Gitea的代码。

对于我们在用到的Xorm Reverse，居然官网只有两句话！（认真的吗Bro）
<img src = "https://pub-2f039bcc31c44eb8a3d5406db7997c4c.r2.dev/截屏2025-02-16 下午11.14.56.png" alt = "如此简洁的文档">

即使提供了Gitea的链接，我们直接使用其提供的最简洁的代码模版也是无法成功生成的。最关键的是没有任何提示信息，就， 啥都不会发生，没任何报错啥的。希望官方能修复一下这一点，并提供更加详尽的说明以供大家debug。

## 最终解决方案
最后我是从这位的大佬的[博客](https://shagain.club/index.php/archives/695/)中找到了解决方式。这里建议也不要直接用官方文档提供的代码下载Xorm Reverse这一部分，笔者反正花时间搞了很久才一步步解决了各种报错。建议直接去Gitea下载可执行文件就行。

另外一个问题就是这个custom.yml文件。如果你按照官网给的极简示例，那大概率是创建不成功的。官网给出的最简洁模版如下：

```yml
kind: reverse
name: mydb
source:
  database: sqlite3
  conn_str: '../testdata/test.db'
targets:
- type: codes
  language: golang
  output_dir: ../models
```

我试了很多次都不行，要像这样：

```yml
kind: reverse
name: mydb
source:
  database: mysql
  conn_str: "user:password@tcp(MysqlIP:MysqlPort)/dbname?charset=utf8"
targets:
- type: codes
  include_tables: # tables included, you can use **
    - "*"
  exclude_tables: # tables excluded, you can use **
    - c
  table_mapper: snake # how table name map to class or struct name
  column_mapper: snake # how column name map to class or struct field name
  table_prefix: "" # table prefix
  multiple_files: true # generate multiple files or one
  language: golang
  template: | # template for code file, it has higher perior than template_path
    package models

    {{$ilen := len .Imports}}
    {{if gt $ilen 0}}
    import (
      {{range .Imports}}"{{.}}"{{end}}
    )
    {{end}}

    {{range .Tables}}
    type {{TableMapper .Name}} struct {
    {{$table := .}}
    {{range .ColumnsSeq}}{{$col := $table.GetColumn .}}    {{ColumnMapper $col.Name}}    {{Type $col}} `{{Tag $table $col}}`
    {{end}}
    }

    func (m *{{TableMapper .Name}}) TableName() string {
        return "{{$table.Name}}"
    }
    {{end}}
  template_path: ./template/goxorm.tmpl # template path for code file, it has higher perior than template field on language
  output_dir: ./models # code output directory
```
将cutsom.yml按照此进行修改（当然记得将里面的参数换成你自己要连接的数据库的具体参数），再次执行我们下载的可执行文件，这样就没问题了。