node-schedule
=============
node-schedule is a cron-like and not-cron-like job scheduler for Node. It allows you to schedule jobs (arbitrary functions) for execution at specific dates, with optional recurrence rules.

node-schedule is for time-based scheduling, not interval-based scheduling. While you can easily bend it to your will, if you only want to do something like "run this function every 5 minutes", you'll find `setInterval` much easier to use, and far more appropriate. But if you want to, say, "run this function at the :20 and :50 of every hour on the third Tuesday of every month," you'll find that node-schedule suits your needs better.

Jobs and Scheduling
-------------------
Every scheduled job in node-schedule is represented by a `Job` object. You can create jobs manually, then execute the `schedule()` method to apply a schedule, or use the convenience function `scheduleJob()` as demonstrated below.

Date-based Scheduling
---------------------
Say you very specifically want a function to execute at 5:30am on December 21, 2012.

	var schedule = require('node-schedule');
	var date = new Date(2012, 11, 21, 5, 30, 0);
	
	var j = schedule.scheduleJob(date, function(){
		console.log('The world is going to end today.');
	});

If you later rethink your decision, you can invalidate the job with the `cancel()` method:

	j.cancel();

Recurrence Rule Scheduling
--------------------------
TODO

Cron-style Scheduling
---------------------
TODO
