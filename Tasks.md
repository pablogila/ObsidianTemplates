# Tasks pendientes

Opened checkboxes under [[#Tasks]] (-to-*do*) and [[#Questions]] (-to-*ask*) headers.

## Tasks
```dataview
TASK
FROM "Weekly journal"|"Meetings"
WHERE !completed
WHERE meta(header).subpath = "Tasks"
GROUP BY file.name
```

