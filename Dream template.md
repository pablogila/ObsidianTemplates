<%*
dream_folder = "Diario/Dreams"
title = tp.file.title;
year = tp.date.now("YYYY");
date = tp.date.now();
file = "Dream " + date;
folder = dream_folder + "/Dreams " + year + "/" + file;
if (title.startsWith("Untitled")){
	title = file;
    await tp.file.rename(`${title}`);
    tp.file.move(folder);
}
tR += `# ${title}\n`;
%>


---

#Lucidez⭐⭐⭐⭐⭐
#Claridad⭐⭐⭐⭐⭐
>**Disparador:**  

