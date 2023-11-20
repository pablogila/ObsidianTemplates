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
date = tp.date.now("YY-MM-DD-") + final_hour + "." + final_minutes;
title = tp.file.title;
if (title.startsWith("Untitled")){
	title = "Meeting " + date;
    await tp.file.rename(`${title}`);
}
tR += `# ${title}\n`;
tR += `[[${week}]]`
%>
__People:__ 
## Discussion



## Tasks
- [ ] 

