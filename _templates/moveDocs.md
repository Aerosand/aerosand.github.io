<%*
function cleanFolderPath(path) {
  return path?.trim().replace(/\/+$/, "") ?? "";
}

// Step 1: 清洗文件名并重命名
const cleanTitle = tp.user.getTitleSnippet(tp.file.title);
await tp.file.rename(cleanTitle);

// Step 2: 选择父目录
const parentOptions = ["Docs", "News"];
const parentChoice = await tp.system.suggester(parentOptions, parentOptions);
let parentFolder = "";

switch (parentChoice) {
  case "Docs":
    parentFolder = "content/docs";
    break;
  case "News":
    parentFolder = "content/News";
    break;
}

let subFolder = await tp.system.prompt(`Subfolder name under "${parentChoice}" (Enter blank or close):`, "");
subFolder = cleanFolderPath(subFolder || "");

let finalFolder = parentFolder;
if (subFolder) {
  finalFolder = parentFolder ? `${parentFolder}/${subFolder}` : subFolder;
}

if (finalFolder) {
  const folderExists = await app.vault.adapter.exists(finalFolder);
  if (!folderExists) {
    await app.vault.createFolder(finalFolder);
    new Notice("📁 Folder created: " + finalFolder);
  } else {
    new Notice("📁 Folder existed: " + finalFolder);
  }
} else {
  new Notice("📁 Vault root");
}

const newPath = finalFolder ? `${finalFolder}/${cleanTitle}` : cleanTitle;
await tp.file.move(newPath);
-%>
