<%*
const newName = (tp.file.title.includes("未命名") || tp.file.title.toLowerCase().includes("untitled"))
    ? await tp.system.prompt("请输入要创建的文件名")
    : tp.file.title;

await tp.file.rename(newName);
%>

title: 🥑<% tp.file.title %>
created_date: <% tp.file.creation_date() %>
updated_date: <% tp.file.last_modified_date() %>
type: 股票
tags:#<% tp.file.creation_date("YYYY-MM") %> #股票 

rating:<% tp.system.suggester(["A", "B", "C"], ["A", "B","C"], true,rating)\>
"status')

