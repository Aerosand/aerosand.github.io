<%-*
// 检查并获取有效标题
async function getValidTitle() {
  let currentTitle = tp.file.title;
  
  // 检查标题是否为空或为默认标题
  const invalidTitles = ["untitled", "未命名", "新文档"];
  const isInvalid = !currentTitle || invalidTitles.some(invalid => 
    currentTitle.toLowerCase().includes(invalid.toLowerCase())
  );
  
  if (isInvalid) {
    // 提示用户输入标题
    const userInput = await tp.system.prompt("文件名称不能为空，请输入名称：");
    
    if (userInput && userInput.trim() !== "") {
      // 使用用户输入的标题
      currentTitle = userInput.trim();
    } else {
      // 使用时间戳作为默认标题
      const now = new Date();
      const timestamp = now.toISOString()
        .replace(/[:.-]/g, '')
        .slice(0, 14);
      currentTitle = `文档_${timestamp}`;
    }
    
    // 使用 Templater 的重命名功能
    await tp.file.rename(currentTitle);
  }
  
  return currentTitle;
}

// 第一步：获取有效标题
const validTitle = await getValidTitle();

// 第二步：提取文件名前缀（"-"之前的部分）并转换为小写
const fileNamePrefix = validTitle.includes("-") 
    ? validTitle.split("-")[0].toLowerCase().trim()
    : "";

// 第三步：根据文件名前缀选择模板
if (fileNamePrefix === "docs") {
    await tp.file.include("[[moveDocs]]");
} else if (fileNamePrefix === "news") {
    await tp.file.include("[[moveNews]]");
} else {
    // 无前缀时使用默认模板
    await tp.file.include("[[1_selectFolderFromClick]]");
}
-%>