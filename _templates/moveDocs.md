<%-*
// 文件夹路径清理函数
function cleanFolderPath(path) {
  return path?.trim().replace(/\/+$/, "") ?? "";
}

// 直接使用当前文件名
const title = tp.file.title;

// 选择父目录
const parentOptions = ["OFSB", "OFSP", "OFST", "OFSC", "OFSR"];
const parentChoice = await tp.system.suggester(parentOptions, parentOptions);
let parentFolder = "";

switch (parentChoice) {
  case "OFSB":
    parentFolder = "content/docs/ofsb";
    break;
  case "OFSP":
    parentFolder = "content/docs/ofsp";
    break;
  case "OFST":
    parentFolder = "content/docs/ofst";
    break;
  case "OFSC":
    parentFolder = "content/docs/ofsc";
    break;
  case "OFSR":
    parentFolder = "content/docs/ofsr";
    break;
}

let subFolder = await tp.system.prompt(`在"${parentChoice}"下的子文件夹名称（留空或关闭）：`, "");
subFolder = cleanFolderPath(subFolder || "");

let finalFolder = parentFolder;
if (subFolder) {
  finalFolder = parentFolder ? `${parentFolder}/${subFolder}` : subFolder;
}

if (finalFolder) {
  const folderExists = await app.vault.adapter.exists(finalFolder);
  if (!folderExists) {
    await app.vault.createFolder(finalFolder);
    new Notice("📁 文件夹已创建: " + finalFolder);
  } else {
    new Notice("📁 文件夹已存在: " + finalFolder);
  }
} else {
  new Notice("📁 库根目录");
}

const newPath = finalFolder ? `${finalFolder}/${title}` : title;
await tp.file.move(newPath);
-%>