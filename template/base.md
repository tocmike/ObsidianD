标题: <% tp.file.title %>
创建时间: <% tp.file.creation_date() %>
修改时间: <% tp.file.last_modified_date() %>
tags:
备注:
其他:
---
<% tp.file.cursor(13) %>
<% await tp.file.move("/学习阅读/阅读/整理笔记" + ((tp.file.title.includes("未命名") || tp.file.title.toLowerCase().includes("untitled")) ? (await tp.system.prompt("请输入要创建的文件名")) : tp.file.title)) %>