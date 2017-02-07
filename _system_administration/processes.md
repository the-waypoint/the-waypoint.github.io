---
---

## Chronological

Listing Processes
* `ps u`
  * prints which programs are running, resources being used and who is running them.
  * historically a 'terminal' represents a single person at a single screen which can only be navigated by inputting characters; _now it is possible to have several terminals on one screen by opening multiple virtual terminal windows on a desktop_
  * Under STAT, R indicates running, S indicates sleeping and (+) denotes a foreground operation.
  * %CPU and %MEM shows the processes' consumption in relation to CPU and RAM
  * VSZ, virtual set size, shows the size of the image process (in kilobytes)
  * RSS, resident set size, shows the size of the program in memory
  * The values of VSZ and RSS may differ because the former refers to the memory allocated for that process and the latter how much memory is actually allocated.
