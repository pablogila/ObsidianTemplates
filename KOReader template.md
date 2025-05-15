<%*
// This template parses JSON files with KOReader highlights
// into Obsidian markdown notes, using the templater plugin.
// The header level is obtained from the highlight color,
// from higher to lower: 'red', 'orange', 'yellow', 'green'.
// Bold text is achieved with the 'invert' style.
input_path = await tp.system.prompt("Path to the KOReader JSON file:", null, true);
// Parse the JSON file
fs = require("fs");
content = fs.readFileSync(input_path, "utf-8");
data = JSON.parse(content);
// Rename the file
file = app.workspace.getActiveFile();
name = `${data.title} - ${data.author} - ebook`;
new_path = `${file.parent.path}/${name}.md`;
await app.fileManager.renameFile(file, new_path);
// Book metadata
output = `---\n`
output += ``
output += `---\n`
// Generate markdown
output += `# ${data.title}\n> ${data.author}\n\n`;
for (entry of data.entries) {
  if (entry.text) {
    // Clean the text
    text = entry.text
    .replace(/\[/g, "\(")
    .replace(/\]/g, "\)");
    // Handle headers
    header = {
      red: "## ",
      orange: "### ",
      yellow: "#### ",
      green: "##### "
    }[entry.color] || "";
    // Formatting
    if (entry.drawer === "invert") {  // Bold text
      output += `**${text}**\n`; 
    } else if (header) {  // Remove \n from headers
      output += `${header}${text.replace(/\n/g, " ")}\n`; 
    } else {  // Regular text
      output += `${text}\n`; 
    }
    if (entry.note) {  // Optional note
      output += `> ${entry.note.replace(/\n/g, " ")}\n`;
    }
    output += `\n`
  }
}
// Update file content
await app.vault.modify(file, output);
%>