# Mood

```dataviewjs
/*
Create a mood heatmap calendar from your daily notes diary with the *Dataview* and *Heatmap Calendar* Obsidian plugins.
Remember to set the CONFIG constants to your use case.
It parses the daily notes of your diary `folder` and determines the mood from the specified `moodTags`, applying the defined color `palette`.
Missing days are determined from the next available daily note;
e.g. if there are entries for 2024-01-01 and 2024-01-03, it assumes the note 2024-01-03 contains the entry for both days 02 and 03.
If more than one mood tag is used per entry, the mood is equally distributed among the corresponding days.
*/
// Configuration
const CONFIG = {
    folder: "Diario",
    palette: {"colors":[
        "#841827",
        "#d03e2b",
        "#dede85",
        "#6c9d3e",
        "#156626"
        ]},
    moodTags: { 
        "#Mood_Genial": 4, 
        "#Mood_Bien":   3, 
        "#Mood_Normal": 2, 
        "#Mood_Mal":    1, 
        "#Mood_Fatal":  0 
    }
};
// Helper functions
const getMoodValues = (page) => {
    if (!page) return [2]; // Default to 'Normal' if page is null
    const values = Object.entries(CONFIG.moodTags)
        .filter(([tag]) => page.file.tags.includes(tag))
        .map(([, val]) => val);
    return values.length ? values : [2];
};
const getAverage = (arr) => arr.reduce((sum, val) => sum + val, 0) / arr.length;
// Main logic
const journalPages = dv.pages(Object.keys(CONFIG.moodTags).join(" OR "))
    .where(p => p.file.folder.includes(CONFIG.folder))
    .sort(p => p.file.name);
const calendarData = [];
let lastProcessedDate = null;
for (let i = 0; i < journalPages.length; i++) {
    const currentPage = journalPages[i];
    const nextPage = journalPages[i + 1];
    // Calculate Look-Ahead Target
    const currentMoods = getMoodValues(currentPage);
    const nextMoods = getMoodValues(nextPage);
    const targetIntensity = getAverage(nextMoods);
    // Sort Moods: Furthest from target -> Closest to target
    currentMoods.sort((a, b) => Math.abs(b - targetIntensity) - Math.abs(a - targetIntensity));
    // Determine Date Range (Backfill from previous entry)
    const currentDate = moment(currentPage.file.name);
    const dateRange = [];
    // Start from day after last entry, or today if it's the first entry
    let iterator = lastProcessedDate ? lastProcessedDate.clone().add(1, 'd') : currentDate.clone();
    while (iterator.isSameOrBefore(currentDate)) {
        dateRange.push(iterator.clone());
        iterator.add(1, 'd');
    }
    // Distribute Moods across the Date Range
    dateRange.forEach((date, index) => {
        // Map day index to mood index proportionally
        const moodIndex = Math.floor(index * (currentMoods.length / dateRange.length));
        const safeIndex = Math.min(moodIndex, currentMoods.length - 1);
        
        calendarData.push({ 
            date: date.format("YYYY-MM-DD"), 
            intensity: currentMoods[safeIndex] 
        });
    });
    lastProcessedDate = currentDate;
}
// Rendering
const uniqueYears = [...new Set(calendarData.map(e => e.date.substring(0, 4)))].sort();
uniqueYears.forEach(year => {
    renderHeatmapCalendar(this.container, {
        year: parseInt(year),
        colors: CONFIG.palette,
        showCurrentDayBorder: false,
        intensityScaleStart: 0,
        intensityScaleEnd: 4,
        entries: calendarData.filter(e => e.date.startsWith(year))
    });
});
```

