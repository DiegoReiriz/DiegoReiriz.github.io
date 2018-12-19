---
title: Apache airflow
image:  /images/flinkzeppelin/squirrelparty.jpg
category: scheduler
tags: [apache, airflow, incubator, etl, orchestrator, scheduler, monitor, monitoring]
---

This are my Apache Airflow notes.

## INTRO

Soooo, WTF is Apache Airflow, let's keep it simple, Apache Airflow it's a Scheduler

 
## Installation

To install Apache Airflow with all it's plugins you only need to perform the following pip command

**NOTE from docs**: GPL dependency -- One of the dependencies of Apache Airflow by default pulls in a GPL library (‘unidecode’). In case this is a concern you can force a non GPL library by issuing export `SLUGIFY_USES_TEXT_UNIDECODE=yes` and then proceed with the normal installation. Please note that this needs to be specified at every upgrade. Also note that if unidecode is already present on the system the dependency will still be used.

```bash

# (optional) 
export AIRFLOW_HOME=~/airflow

# only if you don't have mysql_config on your system
apt install default-libmysqlclient-dev

pip install "apache-airflow[jdbc,mssql,mysql,slack,ssh,kubernetes]"
```

So if everything worked as it should, you should be able to run the following command to obtain the CLI documentation

```
airflow -h

INFO - Using executor SequentialExecutor
usage: airflow [-h]
               {backfill,list_tasks,clear,pause,unpause,trigger_dag,delete_dag,pool,variables,kerberos,render,run,initdb,list_dags,dag_state,task_failed_deps,task_state,serve_logs,test,webserver,resetdb,upgradedb,scheduler,worker,flower,version,connections,create_user}
               ...

positional arguments:
  {backfill,list_tasks,clear,pause,unpause,trigger_dag,delete_dag,pool,variables,kerberos,render,run,initdb,list_dags,dag_state,task_failed_deps,task_state,serve_logs,test,webserver,resetdb,upgradedb,scheduler,worker,flower,version,connections,create_user}
                        sub-command help
    backfill            Run subsections of a DAG for a specified date range.
                        If reset_dag_run option is used, backfill will first
                        prompt users whether airflow should clear all the
                        previous dag_run and task_instances within the
                        backfill date range.If rerun_failed_tasks is used,
                        backfill will auto re-run the previous failed task
                        instances within the backfill date range.
    list_tasks          List the tasks within a DAG
    clear               Clear a set of task instance, as if they never ran
    pause               Pause a DAG
    unpause             Resume a paused DAG
    trigger_dag         Trigger a DAG run
    delete_dag          Delete all DB records related to the specified DAG
    pool                CRUD operations on pools
    variables           CRUD operations on variables
    kerberos            Start a kerberos ticket renewer
    render              Render a task instance's template(s)
    run                 Run a single task instance
    initdb              Initialize the metadata database
    list_dags           List all the DAGs
    dag_state           Get the status of a dag run
    task_failed_deps    Returns the unmet dependencies for a task instance
                        from the perspective of the scheduler. In other words,
                        why a task instance doesn't get scheduled and then
                        queued by the scheduler, and then run by an executor).
    task_state          Get the status of a task instance
    serve_logs          Serve logs generate by worker
    test                Test a task instance. This will run a task without
                        checking for dependencies or recording its state in
                        the database.
    webserver           Start a Airflow webserver instance
    resetdb             Burn down and rebuild the metadata database
    upgradedb           Upgrade the metadata database to latest version
    scheduler           Start a scheduler instance
    worker              Start a Celery worker node
    flower              Start a Celery Flower
    version             Show the version
    connections         List/Add/Delete connections
    create_user         Create an admin account

optional arguments:
  -h, --help            show this help message and exit

```

### Initiating Airflow Database
Airflow requires a database to be initiated before you can run tasks. If you’re just experimenting and learning Airflow, you can stick with the default SQLite option. If you don’t want to use SQLite, then take a look at Initializing a Database Backend to setup a different database.

After configuration, you’ll need to initialize the database before you can run tasks:

```
airflow initdb
```

### Starting the web server

The folowing command starts a [web server](http://localhost:8080)

```
airflow webserver
```

This web server should contain a list of example DAGs(works that the platform runs)


![web server landing page]()

### Staring a scheduler 

```
airflow scheduler
```


## WHAT IT CONTAINS MUDERLURKER

## Performing Something
