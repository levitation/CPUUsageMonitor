## Process CPU Usage Monitor
A program to monitor CPU and other resource usage of processes. The program exits when some resource usage metrics exceed a specified treshold.

## Use cases
If there is some process that occasionally misbehaves and starts hogging computer resources then you can use CPU Usage Monitor tool to launch corrective actions upon detection of the occurrence of the problem. 

You can create a batch file which contains a loop and inside that loop two commands: the first command starts the CPU Usage Monitor tool. If the first command quits then that means that the trigger situation has been detected and therefore it is appropriate time for the second command to be executed. The second command would contain some corrective action. For example, the second command could:
	a) Kill and restart the misbehaving process.
	b) Send an email to the administrator.

Example batch file content:
	@echo off
	:s
	CPUUsageMonitor.exe -cpuUsageTreshold=90 -handlesTreshold=75000 -memoryCommitTresholdMB=1024 -programRegEx="YourProblematicExecutableName"
	pskill.exe "YourProblematicExecutableName"
	sleep 1
	goto s

The above example monitors two resource usage metrics of a process called "YourProblematicExecutableName". Once ANY of the CPU usage percent metric, handle usage count metric, or the committed memory usage metric exceed the corresponding treshold, the process will be killed by the next command in the batch file. By default the trigger activates (that is, CPU Usage Monitor quits) when some of the thresholds is exceeded for 3 consequtive checks with 5 second intervals, and then the resource usage violation continues for another 30 seconds after that. If the monitored process resumes normal resource usage during that additional time interval then the trigger is reset and CPU Usage Monitor continues running without quitting.

### State
Ready to use. Maintained and in active use.

### Program arguments and their default values
-help (Shows help text)
-failIfNotResponding=True (Consider a process as failed when it is not responding.)
-cpuUsageTreshold= (Cpu usage treshold percent (inclusive) at which a program is considered as failed.)
-memoryCommitTresholdMB= (Memory commit treshold in MB (inclusive) at which a program is considered as failed.)
-workingSetTresholdMB= (Working set treshold in MB (inclusive) at which a program is considered as failed.)
-gdiHandlesTreshold= (GDI handles treshold (inclusive) at which a program is considered as failed.)
-userHandlesTreshold= (User handles treshold (inclusive) at which a program is considered as failed.)
-handlesTreshold= (Handles treshold (inclusive) at which a program is considered as failed.)
-pagedPoolTresholdKB= (Paged pool treshold in KB (inclusive) at which a program is considered as failed.)
-nonPagedPoolTresholdKB= (NonPaged pool treshold in KB (inclusive) at which a program is considered as failed.)
-applyCpuUsageTresholdPerCpu=True (Whether the Cpu Usage Treshold percent applies to capability of one cpu or capability of all cpu-s.)
-outageTimeBeforeGiveUpSeconds=30 (How long outage should last before trigger is activated and CPUUsageMonitor quits. NB! This timeout starts only after the failure count specified with -outageConditionNumChecks has been exceeded.)
-outageConditionNumChecks=3 (How many checks should fail before outage can be declared)
-passedCheckIntervalMs=10000 (How many ms to pause after a successful check)
-failedCheckIntervalMs=5000 (How many ms to pause after a failed check)
-programRegEx=\"regular expression\" (Regular expression with process name. NB! Do not include file extension .exe! If regular expression matches multiple processes then if at least one process matching the given expression exceeds the cpu usage limit then the expression is considered as failed. Multiple regular expressions can be specified. In this case the program exits only after ALL monitored regular expressions have failed.)
-failIfNoMatchingProcesses=False (Consider having no matching processes as a failure too. Failure is still declared only after outage condition counts and timeouts have passed.)
-executeCommandOnFail="" (Instead of quitting the monitor program, execute a command specified in this argument, and continue running the monitor program. To add execution arguments to this call, this argument must be the last one of the monitor arguments.)


[![Analytics](https://ga-beacon.appspot.com/UA-351728-28/CPUUsageMonitor/README.md?pixel)](https://github.com/igrigorik/ga-beacon)    
