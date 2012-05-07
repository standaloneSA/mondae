mondae
======

Provides an interface for application-requested monitoring


Design
------

Mondae is a daemon written in Ruby that is installed on machines that have software needing to be monitored by thir party software, such as Nagios. 

Software can register itself with mondae and provide an arbitrary number of metrics to be monitored. In addition to the metric names, the method of acquiring those metrics is defined, as is a suggested threshold value for warning and critical alerts. Of course, the monitoring system can override these suggested values.

The applications' configurations are stored in /etc/mondae/conf.d/applicationname.json in JSON format. The following is an example of an application configuration: 

```javascript
object { 
	name: 	"Application Name", 
	object {
		name: 			"Metric 1", 
		commandLine: 	"/path/to/instrumentation", 
		user: 			"applicationToSudo", 
		warnThresh:		"10", 
		critThresh:		"20", 
		administrator:	"bob@mycorp.com", 
		}  
}
```

There will certainly be refinements to the object definition as time goes on.

Monitoring Side
---------------

The monitoring system will need to be made aware of mondae through the normal technique of built-in checks (or the standard plugin interface). 

The limitation of most current monitoring systems will become apparent as new applications are deployed and registered with mondae. Most current monitoring systems don't allow for dynamic object creation, and instead require the monitoring service to be restarted after configuration writing. In this case, the monitoring server will need to keep mondae in an abstracted service and parse out the various statuses independently. 

