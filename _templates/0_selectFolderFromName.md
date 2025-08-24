<%-*
// 提取文件名前缀（"-"之前的部分）并转换为小写
const fileNamePrefix = tp.file.title.includes("-") 
    ? tp.file.title.split("-")[0].toLowerCase().trim()
    : "";

// 根据文件名前缀选择模板
if (fileNamePrefix === "docs") {
    await tp.file.include("[[moveDocs]]", null, true);
} else if (fileNamePrefix === "news") {
    await tp.file.include("[[moveNews]]", null, true);
} else {
    // 无前缀时使用默认模板
    await tp.file.include("[[1_selectFolderFromClick]]", null, true);
}
-%>

