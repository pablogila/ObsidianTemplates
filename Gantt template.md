```mermaid
gantt
title TITLE
dateFormat YYYY-MM-DD

section FIRST
	task1 : crit, 2100-01-01, 2d
	task2 : 1d

section SECOND
	task3 : done, X, 2100-01-01, 1d
	task3 : active, after X, 1d
```
