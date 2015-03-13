# mesos-task-exporter

Export metrics of tasks running on [Apache Mesos](http://mesos.apache.org/) to [Prometheus](http://prometheus.io/).

## Features

* Auto-discover of new slaves
* Auto-discover of new tasks
* Stateless - can run anywhere
* Framework-independent

## Metrics

### Exported metrics

* `mesos_task_cpus_system_time_seconds`
* `mesos_task_cpus_system_usage`
* `mesos_task_cpus_total_usage`
* `mesos_task_cpus_user_time_seconds`
* `mesos_task_cpus_user_usage`
* `mesos_task_mem_limit_bytes`
* `mesos_task_mem_rss_bytes`

Metrics with the suffix `_usage` are calculated in the same way as done by the Mesos WebUI.

### Labels

Every metric has the following labels attached to it:

* `executor_id` - The unique ID of the executor
* `framework` - The name of the framework that spawned the task
* `slave_pid` - The PID of the Mesos slave that the task is running on as exposed by the `/master/state.json` endpoint of the Mesos master
* `task` - The name of the task

#### [Chronos](https://github.com/mesos/chronos) example

```
mesos_task_cpus_system_time_seconds{executor_id="ct:1426247880000:0:examplejob:",framework="chronos-2.3.2_mesos-0.20.1-SNAPSHOT",slave_pid="slave(1)@10.168.1.11:5051",task="ChronosTask:examplejob"} 0.02
```

#### [Marathon](https://github.com/mesosphere/marathon) example

```
mesos_task_cpus_system_time_seconds{executor_id="com_example_redis.b8f17462-c96c-11e4-b9ff-56847afe9799",framework="marathon",slave_pid="slave(1)@10.168.1.11:5051",task="redis.example.com"} 10.71
```

## Configuration

```
$ ./mesos-task-exporter -h
Usage of ./mesos-task-exporter:
  -exporter.address=":55555": Address of the exporter
  -exporter.endpoint="/metrics": Path where metrics are served
  -log.level="info": Log level
  -mesos.master-pollinterval=15s: Interval to poll the Mesos master leader for new slaves
  -mesos.masters="http://localhost:5050": A list of Mesos masters separated by commas
  -mesos.slave-pollinterval=15s: Interval to poll a Mesos slave for stats of tasks
```

## Deployment


