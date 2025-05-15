<%*
week_past = "Week " + tp.date.weekday("YYYY-MM-DD", -6);
week_present = "Week " + tp.date.weekday("YYYY-MM-DD", 1);
week_next = "Week " + tp.date.weekday("YYYY-MM-DD", 8);
week_future = "Week " + tp.date.weekday("YYYY-MM-DD", 15);
title = tp.file.title;
year = tp.date.weekday("YYYY", 1);
file = week_present
folder = "Work ⚛️/Weekly journal/Weeks " + year + "/" + file;
if (title.startsWith("Untitled")){
	title = week_present;
    await tp.file.rename(`${title}`);
    tp.file.move(folder)
}
tR += `# ${title}\n`;
if (title.startsWith(week_present)){
	tR += `< [[${week_past}]] | [[${week_next}]] >`}
if (title.startsWith(week_next)){
	tR += `< [[${week_present}]] | [[${week_future}]] >`}
%>

---
[[Tasks pendientes]]
## Tasks
- [ ] 
## Questions
- [ ] 
---
## Calendar [+](https://calendar.google.com/calendar/u/0/r)
- 
---
## Journal

