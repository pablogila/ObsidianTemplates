<%*
week_past = "Week " + tp.date.weekday("YYMMDD", -6);
week_present = "Week " + tp.date.weekday("YYMMDD", 1);
week_present_ = "Week " + tp.date.weekday("YYYY-MM-DD", 1);
week_next = "Week " + tp.date.weekday("YYMMDD", 8);
week_next_ = "Week " + tp.date.weekday("YYYY-MM-DD", 8);
week_future = "Week " + tp.date.weekday("YYMMDD", 15);
title = tp.file.title;
year = tp.date.weekday("YYYY", 1);
file = week_present
folder = "Weekly journal/Weeks " + year + "/" + file;
if (title.startsWith("Untitled")){
	title = week_present;
    await tp.file.rename(`${title}`);
    tp.file.move(folder)
}
if (title.startsWith(week_present)){
	tR += `# ${week_present_}\n< [[${week_past}]] | [[${week_next}]] >`}
if (title.startsWith(week_next)){
	tR += `# ${week_next_}\n< [[${week_present}]] | [[${week_future}]] >`}
if (title.startsWith(week_future)){
	tR += `# Ha, nice try ;)`}
	
%>

---
[[Tasks pendientes]]
## Tasks
- [ ] 
---
## Calendar [+](https://calendar.google.com/calendar/u/0/r)
- 
---
## Journal

