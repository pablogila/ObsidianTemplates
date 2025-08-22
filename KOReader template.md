<%*
// This template imports JSON files with your KOReader highlights
// into Obsidian markdown notes, using the Obsidian Templater plugin.
//
// Just create an empty note and apply this template.
// It will ask for the JSON path in your computer.
// Be careful, it will overwrite the current note!
//
// The format of the notes can be modified by changing the highlight color:
// From higher to lower, the header level is specified
// with the colors 'red', 'orange', 'yellow', 'green'.
// Bold text is achieved with 'purple' color.
// Item lists for every new line are achieved with 'blue' color.
//
// Function to convert UNIX dates to yyyy-mm-dd
function unixToDate(timestamp) {
  date_js = new Date(timestamp * 1000);  // JS dates use milliseconds
  date_year = date_js.getFullYear();
  date_month = String(date_js.getMonth() + 1).padStart(2, '0');
  date_day = String(date_js.getDate()).padStart(2, '0');
  return `${date_year}-${date_month}-${date_day}`;
  }
// Prompt and parse JSON file path
input_path = await tp.system.prompt("Path to the KOReader JSON file:", null, true);
fs = require("fs");
content = fs.readFileSync(input_path, "utf-8");
data = JSON.parse(content);
// Rename the file
file = app.workspace.getActiveFile();
name = `${data.title} - ${data.author} - ebook`;
new_path = `${file.parent.path}/${name}.md`;
await app.fileManager.renameFile(file, new_path);
// Get modified timestamp
timestamp_modified = data.created_on;  // UNIX time
date = unixToDate(timestamp_modified);
// Get first and last timestamps
all_timestamps = data.entries.map(entry => entry.time);  // UNIX time
timestamp_min = Math.min(...all_timestamps);
timestamp_max = Math.max(...all_timestamps);
date_min = unixToDate(timestamp_min)
date_max = unixToDate(timestamp_max)
// Book metadata
output = `---\n`
output += `title: ${data.title}\n`
output += `author: ${data.author}\n`
output += `first: ${date_min}\n`
output += `last: ${date_max}\n`
// output += `modified: ${date}\n`
output += `tags: ebook\n`
output += `---\n`
// Book highlights
output += `# ${data.title}\n**${data.author}**\n\n`;
for (entry of data.entries) {
  if (entry.text) {
    // Avoid accidental wikilinks
    text = entry.text
    .replace(/\[/g, "\\[");
    // Custom format from the highlight color
    header = {
      red: "## ",
      orange: "### ",
      yellow: "#### ",
      green: "##### "
    }[entry.color] || "";
    if (header) {  // Remove \n from headers
      output += `${header}${text.replace(/\n/g, " ")}\n`; 
    } else if (entry.color === "purple") {  // Bold text
      output += `**${text}**\n`; 
    } else if (entry.color === "blue") {  // Item list
      output += `- ${text.replace(/\n/g, "\n- ")}\n`; 
    } else {  // Regular text
      output += `${text}\n`; 
    }
    // Optional note
    if (entry.note) {
      output += `> ${entry.note.replace(/\n/g, "\n> ")}\n`;
    }
    output += `\n`
  }
}
// Update file content
await app.vault.modify(file, output);
%>