#LinuxCommands 
The **top** command is used to monitor system processes and resource usage in real-time. The following is a portion of an example output:
```bash
top
top - 14:30:23 up 5:10, 1 user, load average: 0.15, 0.22, 0.28 
Tasks: 213 total, 1 running, 212 sleeping, 0 stopped, 0 zombie 
%Cpu(s): 2.0 us, 1.3 system, 0.0 ni, 96.3 id, 0.3 wa, 0.0 hi, 0.0 si, 0.0 st MiB Mem : 7824.9 total, 2576.8 free, 2935.0 used, 2313.1 buff/cache 
MiB Swap: 2048.0 total, 2048.0 free, 0.0 used. 4558.1 avail Mem 

PID USER PR NI VIRT RES SHR S %CPU %MEM TIME+ COMMAND 
1234 user 20 0 3626108 205008 89380 S 2.0 2.6 0:45.85 gnome-shell 
5678 user 20 0 859492 51528 38060 S 1.3 0.6 0:10.91 Xorg 
9101 user 20 0 722816 36096 29088 S 0.7 0.5 0:05.62 gnome-terminal
```
The display is continuously updated (every three seconds by default).
- The first line shows current time, system uptime, number of users, and load average.
- The second line displays the total amount of tasks and how many are in each state.
- The third shows CPU usage distribution in percentages.
- The fourth and fifth show memory and swap usage.
- The table below displays all individual processes, sorted by CPU usage by default.
To exit the view, press **q**. To see a help screen with all available commands, press **h** or **?**
### Sorting Processes
When in the **top** view, we can change the way processes are sorted by pressing certain keys while uppercase is enabled.
- Pressing **M** sorts the processes by memory usage.
- **P** reverts back to sorting by CPU usage.
- **N** sorts the process by process ID (PID)
- **R** does not change sorting criteria but reverses the current sort order.
### Changing Update Interval (-d)
We can change the default display update interval (3 seconds) using the **-d N** option, where N is the desired interval, in seconds. So, **top -d 0.5** changes the update interval from 3 to 0.5 seconds.
### Display Specific User's Processes (-u)
The command **top -u USER** will show only the processes owned by the provided user. This can be useful when troubleshooting issues related to a specific user's activities or if you want to focus on your own processes in a multi-user system.
### Display Only Active Processes (-i)
Using the command **top -i** will display only active processes, filtering out those with zero CPU usage since the last update. This can be used to identify processes that are currently using resources.