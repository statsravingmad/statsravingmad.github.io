---
title: Notes on custom script execution logging with QlikView


excerpt: My notes on enabling script execution logging in QlikView scripting

categories: 
  - measure
tags: 
  - qlikview
  - logs
  - script
  - notes

---


<section id="table-of-contents" class="toc">
<header>
<h3>Overview</h3>
</header>
<div id="drawer" markdown="1">
*  Auto generated table of contents
{:toc}
</div>
</section><!-- /#table-of-contents -->

## The Problem
One problem we have with the apps and the synchronizers is that we don't have controlled
logging in order to evaluate the performance. This is an issue that surfaces quite often when investigating various issues (*examples-go-here*). We can add some data points towards solving such issues by adding calls into our scripts
using the [QlikView Components](https://github.com/RobWunderlich/Qlikview-Components) library ; a very interesting and useful library with subroutines utilities
that we partially use already (eg for creating a Calendar Table).

## Logging calls
Logging is a means of tracking events that happen when some software runs. The software’s developer adds logging calls to their code to indicate that certain events have occurred. An event is described by a descriptive message which can optionally contain variable data (i.e. data that is potentially different for each occurrence of the event).

### First things first
To start logging we need to make the library available into our scripts.

~~~ SQL
/* Include the qvc.qvs file at the beginning of your script. */
$(Must_Include=path-to-qvc\qvc_runtime\qvc.qvs);
~~~

Then in order to suppress interaction with error windows (ie press OK in each error) set `ErrorMode` to silence.

~~~ SQL
SET ErrorMode=0;
~~~

Just for reference setting it to `1` results into the normal behaviour you expect in each error (press OK to continue execution) and `2` results to break the execution on the first error.

### A Log
It is straightforward to add a logging message in the body of the script like this

```
CALL Qvc.Log('Starting script');
```
### Add attributes to logs
Events also have an importance which the developer ascribes to the event; the importance can also be called the level or severity. We can use three levels of logging

|  Level  |  When it’s used   |
|---------|--------------------|
| `INFO`    | Confirmation that things are working as expected.                                                                                                                      |
| `WARNING` | An indication that something unexpected happened, or indicative of some problem in the near future (e.g. ‘disk space low’). The software is still working as expected. |
| `ERROR`   | Due to a more serious problem, the software has not been able to perform some function.                                                                                |
If omitted, default is `Qvc.Log.v.Level.INFO`.

### QlikView Error Variables
Things we can log in our messages are based on the following variables

1. `ScriptError`
2. `ScriptErrorList`
3. `ScriptErrorDetails`
4. `ScriptErrorCount`

Sometimes you want logging output to contain contextual information in addition to the parameters passed to the logging call (probably using a bit of concatenation to get more comprehensible messages).

### Log once or keep a history?
By default the logging will retain records for 1 day. If we need more days of log to keep in log file we can configure it.

~~~ SQL
SET Qvc.Log.v.KeepDays = 10 /* I will keep 10 days worth of logs */
~~~

### Store logging externally
Except from storing the log messages in the application we can direct the output to a log file.

~~~ SQL
SET Qvc.Log.v.LogFileName = Log_Example_Custom_Name.txt;
SET Qvc.Log.v.WriteLogFile = -1;		/* Turn on external log file writing */
~~~

## An example

### Logging strategy

1. Log who initiates the `LOAD` process by his `OSUser()`
2. For each phase of script (we organize it under `$tab`) we log the start and the end
3. In the end we check to see if the `ScriptErrorDetails` is not `NULL` and log the error messages per phase.
3. For each `LOAD` we log the start and the end of the process
4. In the end we log the list of errors encountered
5. Populate a statistics table for the tables loaded.

Note here that `ScriptErrorDetails` and `ScriptErrorList` are incrementing for each error occurring.

Below you can see how the script has lines for logging phases, steps & in the end it provides a summary of the tables loaded.

~~~ SQL
/* Include the qvc.qvs file at the beginning of your script. */
$(Must_Include=path-to-qvc\qvc_runtime\qvc.qvs);
/* Test the Log routine using the default settings for Log table name and field name. */
CALL Qvc.Log('Starting script');
CALL Qvc.Log( DocumentName() & ' is being reloaded by ' & OSUser());
SLEEP 2000;		/* Sleep 2 seconds */

///$tab Detailed Load
CALL Qvc.Log('Starting Phase 1 :  Detailed Tables Load');
CALL Qvc.Log('Load Searches Details Table');
/* Load the Searches Details Table */
SearchesDetails:
LOAD aid,
     brand,
     ond,
     time_taken,
     id as ws_id,
     date
FROM
C:\Users\m.parzakonis\Desktop\searches_sample.qvd
(qvd);

CALL Qvc.Log('Load Landings Details Table');
/* Capture errors */
IF '$(#ScriptError)'>0 then
CALL Qvc.LogError('Error details: $(ScriptErrorList)'); /* Get a list of errors. */
end IF
/*... more script ommited... */

Call Qvc.LogError('Error details: $(ScriptErrorList)'); /* Get the final list of errors. */
/* Write Table statistics */
Call Qvc.TableStats;	/* No message parameter */
~~~


### Sample Logging output

The following is a sample run of the script

| Qvc.LogMessage               |                                                                            |
|------------------------------|----------------------------------------------------------------------------|
| 00001 05/02/2015 11:15:46 AM | Starting script                                                            |
| 00002 05/02/2015 11:15:46 AM | detailed_aggregated_app.qvw is being reloaded by PAMEDIAKOPES\m.parzakonis |
| 00003 05/02/2015 11:15:48 AM | Starting Phase 1 :  Detailed Tables Load                                   |
| 00004 05/02/2015 11:15:48 AM | Load Searches Details Table                                                |
| 00005 05/02/2015 11:16:41 AM | Load Landings Details Table                                                |
| 00006 05/02/2015 11:16:42 AM | Ending Phase 1 :  Detailed Tables Load                                     |
| 00007 05/02/2015 11:16:42 AM | Starting Phase 2 :  Aggregated Tables Load                                 |
| 00008 05/02/2015 11:16:42 AM | Load Searches Aggr Table                                                   |
| 00009 05/02/2015 11:16:44 AM | Ending Phase 2 :  Aggregated Tables Load                                   |
| 00010 05/02/2015 11:16:44 AM | Starting Phase 3 :  Create dimensions/expressions for ad-hoc reporting     |
| 00011 05/02/2015 11:16:44 AM | Error details: Syntax Error   Syntax Error                                 |
| 00012 05/02/2015 11:16:44 AM | Ending Phase 3 :  Create dimensions/expressions for ad-hoc reporting       |
| 00013 05/02/2015 11:16:44 AM | TableStats Begin:                                                          |
| 00014 05/02/2015 11:16:44 AM | Table=Qvc.LogTable, Rows=12, Fields=2                                      |
| 00015 05/02/2015 11:16:44 AM | Table=SearchesDetails, Rows=104.954.421, Fields=6                          |
| 00016 05/02/2015 11:16:44 AM | Table=LandingsDetails, Rows=1.652.172, Fields=5                            |
| 00017 05/02/2015 11:16:44 AM | Table=SearchesAggr, Rows=15.650.422, Fields=7                              |
| 00018 05/02/2015 11:16:44 AM | Table=expressions, Rows=1, Fields=2                                        |
| 00019 05/02/2015 11:16:44 AM | Table=dimensions, Rows=6, Fields=2                                         |
| 00020 05/02/2015 11:16:44 AM | 6 tables listed.                                                           |
| 00021 05/02/2015 11:16:44 AM | TableStats End:                                                            |

## Summary
Now, we have records to analyze our script modifications towards performance & validity of syntax.

When starting a new application use a lot of logging and as your script gets formulated and ironed out remove a lot of `INFO` recording to the bare minimum. Also, keep in mind that the logging is not capable of catching all errors occurring in the execution unless we have a `Qvc.LogError` call in place.


## Appendix

### Qlikview's standard logging

Qlikview can generate its own log natively. Navigate to `Settings` > `Document properties` > `General` and select Generate log-file. Opt-in for the timestamp in log-file as well.

You can add custom messages in this file using the `TRACE` function, e.g

~~~ SQL
TRACE 'Load Landings Details Table' ;
~~~

### Error codes
QlikView operates on 13 error codes. Traditionally `0` is reserved for `No error`.

- `No error (Script Error=0)`
- `General error (Script Error=1)`
- `Syntax error (Script Error=2)`
- `General ODBC error (Script Error=3)`
- `General OLE DB error (Script Error=4)`
- `General custom database error (Script Error=5)`
- `General XML error (Script Error=6)`
- `General HTML error (Script Error=7)`
- `File not found (Script Error=8)`
- `Database not found (Script Error=9)`
- `Table not found (Script Error=10)`
- `Field not found (Script Error=11)`
- `File has wrong format (Script Error=12)`
- `BIFF error (Script Error=13)`
- `BIFF error encrypted (Script Error=14)`
- `BIFF error unsupported version (Script Error=15)`
- `Semantic error (Script Error=16)`
