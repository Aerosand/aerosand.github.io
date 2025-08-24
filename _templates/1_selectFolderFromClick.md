<%-*
let filetype = await tp.system.suggester(
  ["Docs", "News"],
  ["Docs", "News"],
  false,
  "Select folder"
);

// 如果用户取消选择（关闭弹窗）或者没有输入任何内容，则不插入任何模板
if (filetype === "Docs") {
  await tp.file.include("[[moveDocs]]");
} else if (filetype === "News") {
  await tp.file.include("[[moveNews]]");
}
// 否则什么都不做
-%>
