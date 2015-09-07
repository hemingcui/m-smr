# The evaluation and benchmarking scripts for crane

@author: tianyu chen

## What evaluation have we done?

1. Benchmark performance for apache, clamav, mediatomb and mongoose using 5 plans. We use apache benchmark and clam benchmark.   
2. Checkpoint-restore performance test for apache, clamav, mediatomb, mongoose and mysql. Performance data includes time of checking point filesystem and process (using criu) and restoring them.   
3. Sensitivity data of sched_with_paxos_usleep and sched_with_paxos_max/min.   
4. Leader-election performance.   

## How to run all these evaluation?

### For 1. 

Just run `$MSMR_ROOT/eval-container/run-all.sh` and get your results in the `perf_log` dir. 

Use `grab.py -b perf_log` to generate a detailed report of your run.

You can config the servers and plans in the `run-all.sh` script. 

## For 2.

First, start a server in lxc using `$MSMR_ROOT/eval-container/start-mg.sh`, say:

```bash
./start-mg.sh httpd "$MSMR_ROOT/apps/apache/install/bin/apachectl -f $MSMR_ROOT/apps/apache/install/conf/httpd.conf -k start"
```

Then run:

```bash
./checkpoint-restore.sh checkpoint httpd ./checkpoint
```

Finally restore the process:

```bash
./checkpoint-restore.sh restore httpd checkpoint-<replace_me_with_your_pid>.tar.gz
```

## For 3. 

Just run: 

```bash
./run-eval-usleep.sh 
```

, and:

```bash
./run-eval-maxmin.sh
```

The script does everything for you. View your results in perf_log directory. 

## For 4

```bash
./run.sh configs/new-mongoose.sh 
```

Grep the logs in $HOME/logs on servers for the timestamps. 

