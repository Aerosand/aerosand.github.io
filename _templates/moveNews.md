<%-*
// 直接使用当前文件名
const title = tp.file.title;
const myFilePath = "/content/news/" + title;
await tp.file.move(myFilePath);
-%>