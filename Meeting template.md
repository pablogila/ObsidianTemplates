<%*
week = "Week " + tp.date.weekday("YYYY-MM-DD", 1);
minutes = tp.date.now("mm");
rounded_minutes = Math.round((minutes / 15)) * 15;
final_minutes = rounded_minutes.toString().padStart(2, '0');
hour = parseInt(tp.date.now("HH"));
if (final_minutes == 60){
	hour += 1;
	final_minutes = "00";
}
final_hour = hour.toString().padStart(2, '0');
date = tp.date.now("YYMMDD") + "T" + final_hour + final_minutes;
title = tp.file.title;
year = tp.date.weekday("YYYY", 1);
file = "Meet " + date;
folder = "Meetings/Meetings " + year + "/" + file;
if (title.startsWith("Untitled")){
	title = file;
    await tp.file.rename(`${title}`);
    tp.file.move(folder);
}
tR += `# ${title}\n`;
tR += `[[${week}]]`
%>
__People:__ 
## Talking points
- [ ] 

## Discussion



## Tasks
- [ ] 

