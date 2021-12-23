About Tasks
===========

Tasks are the basic unit to be scheduled.

Basis
-----

* A task is differentiated by its `taskid`. (Default: `md5(url)`, can be changed by overriding the `def get_taskid(self, task)` method)
* Tasks are isolated between different projects.
* A Task has 4 status:
    - active
    - failed
    - success
    - bad - not used
* Only tasks in active status will be scheduled.
* Tasks are served in order of `priority`.

Schedule
--------

#### new task

When a new task (never seen before) comes in:

* If `exetime` is set but not arrived, it will be put into a time-based queue to wait.
* Otherwise it will be accepted.

When the task is already in the queue:

* Ignored unless `force_update`

When a completed task comes out:

* If `age` is set, `last_crawl_time + age < now` it will be accepted. Otherwise discarded.
* If `itag` is set and not equal to it's previous value, it will be accepted. Otherwise discarded.


#### task retry

When a fetch error or script error happens, the task will retry 3 times by default.

The first retry will execute every time after 30 seconds, 1 hour, 6 hours, 12 hours and any more retries will postpone 24 hours.

If `age` is specified, the retry delay will not larger then `age`.

You can config the retry delay by adding a variable named `retry_delay` to handler. `retry_delay` is a dict to specify retry intervals. The items in the dict are {retried: seconds}, and a special key: '' (empty string) is used to specify the default retry delay if not specified.

e.g. the default `retry_delay` declares like:


```
class MyHandler(BaseHandler):
    retry_delay = {
        0: 30,
        1: 1*60*60,
        2: 6*60*60,
        3: 12*60*60,
        '': 24*60*60
    }
```
