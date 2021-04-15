# Appendix. SSH Access to Compute Nodes

If you run a job with the resource type `rt_F`, you can enable SSH login to the compute nodes by specifying options when running `qrsh` or `qsub` command.

| Option | Description |
|:--|:--|
| -l USE\_SSH=*1* | Enable SSH login to the compute nodes. |
| -v SSH\_PORT=*port* | Specify the port number (default: 2222) in the range of 2200-2299. |

If you enable this option, you will be able to login to the compute nodes from the interactive nodes with SSH.
If you are running a job that uses multiple nodes, you will be able to login to each other between compute nodes.

Only the user who submitted the job can login with SSH. And, when the job finishes executing, the session logged in to the compute node is automatically disconnected.

The following is an example of a job script when executing the `qsub` command:

```bash
#!/bin/bash

#$-l rt_F=2
#$-l USE_SSH=1
#$-v SSH_PORT=2299
#$-cwd

...
```

When the job starts running, check the compute nodes assigned with the `qstat` command.

```
[username@es1 ~]$ qstat -j 12345 | grep exec_host_list
exec_host_list        1:    g0001:80, g0002:80
```

As mentioned above, it was confirmed that `g0001` and` g0002` were assigned.

Next, login from the interactive node to the compute node (`es1` -> `g0001`) using the port number specified in the option.

```
[username@es1 ~]$ ssh -p 2299 g0001
[username@g0001 ~]$ 
```

And, you are also able to login to each other between compute nodes (`g0001` -> `g0002`).

```
[username@g0001 ~]$ ssh -p 2299 g0002
[username@g0002 ~]$
```

`qrsh` command can also be executed with `-l USE_SSH=1` option.

```
[username@es1 ~]$ qrsh -g grpname -l rt_F=1 -l USE_SSH=1 -v SSH_PORT=2299
[username@g0001 ~]$ hostname -s
g0001
```

Next, login the interactive node to the compute node with using another terminal.

```
[username@es1 ~]$ ssh -p 2299 g0001
[username@g0001 ~]$ 
```
