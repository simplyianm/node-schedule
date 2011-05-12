node-schedule
=============
node-schedule is a cron-like and not-cron-like job scheduler for Node. It allows you to schedule jobs (arbitrary functions) for execution at specific dates, with optional recurrence rules.

node-schedule is for time-based scheduling, not interval-based scheduling. While you can easily bend it to your will, if you only want to do something like "run this function every 5 minutes", you'll find `setInterval` much easier to use, and far more appropriate. But if you want to, say, "run this function at the :20 and :50 of every hour on the third Tuesday of every month," you'll find that node-schedule suits your needs better.

Note that node-schedule is for in-process scheduling, i.e., scheduled jobs will only fire as long as your script is running, and the schedule will disappear when execution completes. If you need to schedule jobs that will persist even when your script *isn't* running, consider using the actual cron.

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
You can build recurrence rules to specify when a job should recur. For instance, consider this rule, which executes the function every hour at 42 minutes after the hour:

	var schedule = require('node-schedule');
	
	var rule = new schedule.RecurrenceRule();
	rule.minute = 42;
	
	var j = schedule.scheduleJob(rule, function(){
		console.log('The answer to life, the universe, and everything!');
	});

You can also use arrays to specify a list of acceptable values, and the `Range` object to specify a range of start and end values, with an optional step parameter. For instance, this will print a message on Thursday, Friday, Saturday, and Sunday at 5pm:

	var rule = new schedule.RecurrenceRule();
	rule.dayOfWeek = [0, new schedule.Range(4, 6)];
	rule.hour = 17;
	rule.minute = 0;
	
	var j = schedule.scheduleJob(rule, function(){
		console.log('Today is recognized by Rebecca Black!');
	});

It's worth noting that the default value of a component of a recurrence rule is `null` (except for seconds, which is 0 for familiarity with cron). If we did not explicitly set `minute` to 0 above, the message would have instead been logged at 5:00pm, 5:01pm, 5:02pm, ..., 5:59pm. Probably not what you want.

Cron-style Scheduling
---------------------
If you're a fan of cron, you can instead define your recurrence rules using a syntax similar to what you might write in your crontab. For example, the above examples rewritten using this style:

	var schedule = require('node-schedule');
	
	var j = schedule.scheduleJob('42 * * * *', function(){
		console.log('The answer to life, the universe, and everything!');
	});
	
And:

	var j = schedule.scheduleJob('0 17 ? * 0,4-6', function(){
		console.log('Today is recognized by Rebecca Black!');
	});

### Unsupported Cron Features
Currently, `W` (nearest weekday), `L` (last day of month/week), and `#` (nth weekday of the month) are not supported. Also, in the day-of-week field, 7 is currently not recognized as a legal value for Sunday (use 0). Most other features supported by popular cron implementations should work just fine.

It is also entirely possible at this point that constructing a cron string that can *only* exist in the past will cause an infinite loop. This is only possible if a year is specified. If the specified year is a number (i.e., not a range), node-schedule will perform a sanity check before attempting to schedule something in the past.

Installing
----------
You can install using `npm` in the usual way.

	npm install node-schedule
