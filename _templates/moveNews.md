<%-*
var cleanTitle = tp.user.getTitleSnippet(tp.file.title) 
var title = `${cleanTitle}`;
await tp.file.rename(`${title}`);
myFilePath = "/content/news/" +  `${title}`;
await tp.file.move(`${myFilePath}`);
-%>