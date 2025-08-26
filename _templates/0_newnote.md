<%-*
/* ---------- 工具函数 ---------- */
async function sanitizeName(name) {
  return name.trim().replace(/[\\\/:*?"<>|]/g, " ").replace(/\s+/g, " ").trim();
}
function ensureMd(basename) {
  return basename.toLowerCase().endsWith(".md") ? basename : basename + ".md";
}
async function ensureFolder(folder) {
  if (!folder) return "";
  const clean = folder.replace(/\/+$/,"").trim();
  if (!clean) return "";
  const exists = await app.vault.adapter.exists(clean);
  if (!exists) await app.vault.createFolder(clean);
  return clean;
}
async function uniquePath(path) {
  let p = path; let i = 1;
  while (await app.vault.adapter.exists(p)) {
    const dot = p.lastIndexOf(".");
    const base = dot > -1 ? p.slice(0,dot) : p;
    const ext  = dot > -1 ? p.slice(dot)  : "";
    p = `${base} (${i++})${ext}`;
  }
  return p;
}

/* ---------- 1. 获取有效文件名 ---------- */
let title = tp.file.title || "";
const invalid = ["untitled","未命名","新文档"];
const isInvalid = !title || invalid.some(v => title.toLowerCase().includes(v.toLowerCase()));
if (isInvalid) {
  const input = await tp.system.prompt("Input note name：");
  title = (input && input.trim()) ? input.trim() : `Note_${new Date().toISOString().replace(/[:.-]/g,"").slice(0,14)}`;
}
title = await sanitizeName(title);

/* ---------- 2. 文件夹选择 ---------- */
let destFolder = "";
const firstChoice = await tp.system.suggester(
  ["Docs（content/docs）","News（content/news）","Root (/)"],
  ["__DOCS__","content/news","__ROOT__"],
  false,
  "请选择目标文件夹"
);

if (firstChoice === "__DOCS__") {
  // 让用户输入子目录名
  const inputFolder = await tp.system.prompt("Input subfolder（Blank for [content/docs] ）");
  if (inputFolder && inputFolder.trim()) {
    destFolder = `content/docs/${inputFolder.trim()}`;
  } else {
    destFolder = "content/docs";
  }
} else if (firstChoice === "__ROOT__") {
  destFolder = ""; // 根目录
} else {
  destFolder = firstChoice || "";
}

/* ---------- 3. 移动 + 改名 ---------- */
const basename = ensureMd(title);
let finalPath = basename; // 默认仅改名
if (destFolder) {
  const folder = await ensureFolder(destFolder);
  finalPath = `${folder}/${basename}`;
}
finalPath = await uniquePath(finalPath);
await tp.file.move(finalPath);
-%>
