--- 
 
# required metadata 
title: "rx_exec: Run a Function With Different Sets of Arguments" 
description: "Allows distributed execution of a function in parallel across nodes (computers) or cores of a “compute context” such as a cluster." 
keywords: "sql" 
author: "bradsev" 
manager: "jhubbard" 
ms.date: "09/06/2017" 
ms.topic: "reference" 
ms.prod: "microsoft-r" 
ms.service: "" 
ms.assetid: "" 
 
# optional metadata 
ROBOTS: "" 
audience: "" 
ms.devlang: "Python" 
ms.reviewer: "" 
ms.suite: "" 
ms.tgt_pltfrm: "" 
ms.technology: "r-server" 
ms.custom: "" 
 
---

# rx_exec


**Applies to: SQL Server 2017 RC2**


## Usage



```
revoscalepy.rx_exec(function: typing.Callable, args: typing.Union[dict,
    typing.List[dict]] = None, times_to_run: int = -1,
    task_chunk_size: int = None, compute_context=None,
    local_state: dict = {}, callback: typing.Callable = None,
    continue_on_failure: bool = True, **kwargs)
```





## Description

Allows distributed execution of a function in parallel across nodes
(computers) or cores of a “compute context” such as a cluster.


## Arguments


### function

the function to be executed; the nodes or cores on which it
is run are determined by the currently-active compute context and by the
other arguments of rx_exec.


### args

arguments passed to the function ‘function’ each time it is executed.
for multiple executions with different parameters, args should be a list where each element is a dict.
each dict element contains the set of named parameters to pass the function on each execution.


### times_to_run

specifies the number of times ‘function’ should be executed. in
case of different parameters for each execution, the length of args must equal
times_to_run. in case where length of args equals 1 or args is not specified,
function will be called times_to_run number of times with the same arguments.


### task_chunk_size

specifies the number of tasks to be executed per compute element. by submitting tasks
in chunks, you can avoid some of the overhead of starting new python processes over and over. for example,
if you are running thousands of identical simulations on a cluster, it makes sense to specify the
task_chunk_size so that each worker can do its allotment of tasks in a single python process.


### compute_context

a RxComputeContext object. Note, if using RxLocalParallel in a Python script called directly
from the command line, you must use a if __name__ == “__main__”: around your code that calls rx_exec.


### local_state

dictionary with variables to be passed to a RxInSqlServer compute context


### callback

An optional function to be called with the results from each execution
of ‘function’


### continue_on_failure

None or logical value. If True, the default, then if an
individual instance of a job fails due to a hardware or network failure, an
attempt will be made to rerun that job. (Python errors, however, will cause
immediate failure as usual.) Furthermore, should a process instance of a job
fail due to a user code failure, the rest of the processes will continue, and
the failed process will produce a warning when the output is collected.
Additionally, the position in the returned list where the failure occured will
contain the error as opposed to a result.


### **kwargs

Contains named arguments to be passed to function. Alternative to using args param.
For a function foo that takes two arguments named ‘x’ and ‘y’, kwargs can be used like:
rx_exec(function=foo, x=5, y=4) (for a single execution with x=5, y=4)
rx_exec(function=foo, x=[1,2,3,4,5], y=6) (for multiple executions with different values of x and y=6)
rx_exec(function=foo, x=[1,2,3], y=[4,5,6]) (for multiple executions with different values of x and y)
Note: if you want to pass in a list as an argument, you must wrap it in another list. For a function foobar
that takes an argument named ‘the_list’, kwargs can be used like:
rx_exec(function=foobar, the_list=[ [0,1,2,3] ]) (single execution with the_list = [0,1,2,3])
rx_exec(function=foobar, the_list=[ [0,1,2],[3,4,5],[6,7,8] ]) (multiple executions)


## Returns

If local compute context or a waiting compute context is active, a list containing the return
value of each execution of ‘function’ with specified parameters. If a non-waiting compute context is active, a jobInfo
object, or a list of a jobInfo objects for multiple executions.


## See also

`RxComputeContext`,
[`RxLocalSeq`](RxLocalSeq.md),
[`RxInSqlServer`](RxInSqlServer.md),
[`rx_get_compute_context`](rx-get-compute-context.md),
[`rx_set_compute_context`](rx-set-compute-context.md).


## Example



```
###
# Local parallel rx_exec with different parameters for each execution
###
import os
from revoscalepy import RxLocalParallel, rx_set_compute_context, rx_exec, rx_btrees, RxOptions, RxXdfData

sample_data_path = RxOptions.get_option("sampleDataDir")
in_mort_ds = RxXdfData(os.path.join(sample_data_path, "mortDefaultSmall.xdf"))
rx_set_compute_context(RxLocalParallel())
formula = "creditScore ~ yearsEmploy"
args = [
    {'data' : in_mort_ds, 'formula' : formula, 'n_tree': 3},
    {'data' : in_mort_ds, 'formula' : formula, 'n_tree': 5},
    {'data' : in_mort_ds, 'formula' : formula, 'n_tree': 7}
]

if __name__ == "__main__":
    models = rx_exec(rx_btrees, args = args)
    # Alternatively
    models = rx_exec(rx_btrees, data=in_mort_ds, formula=formula, n_tree=[3,5,7])

###
## SQL rx_exec
###

from revoscalepy import RxSqlServerData, RxInSqlServer, rx_exec
formula = "ArrDelay ~ CRSDepTime + DayOfWeek"
connection_string="Driver=SQL Server;Server=.;Database=RevoTestDB;Trusted_Connection=TRUE"
query="select top 100 [ArrDelay],[CRSDepTime],[DayOfWeek] FROM airlinedemosmall"

ds = RxSqlServerData(sql_query = query, connection_string = connection_string)
cc = RxInSqlServer(
    connection_string = connection_string,
    num_tasks = 1,
    auto_cleanup = False,
    console_output = True,
    execution_timeout_seconds = 0,
    wait = True
    )

def remote_call(dataset):
    from revoscalepy import rx_data_step
    df = rx_data_step(dataset)
    return len(df)

results = rx_exec(function = remote_call, args={'dataset': ds}, compute_context = cc)
print(results)
## End(Not run)
```

