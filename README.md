# Remote GPU Monitor

This Python script allows to check for free Nvidia GPUs in remote servers.
Additional features include to list the type of GPUs and who's using them.
The idea is to speed up the work of finding a free GPU in institutions that share multiple GPU servers.

The script works by using your account to SSH into the servers and running `nvidia-smi`. 

## Features

- [X] Show all free GPUs across servers
- [X] Show all current users of all GPUs (-l or --list)
- [X] Show all GPUs used by yourself (-m or --me)
- [ ] Resolve usernames to real names (-f or --finger)

## Requirements

- python3
- SSH access to some Linux servers with Nvidia GPUs
- If the server you connect to uses a different user name than your local name, you either have to specify your name on the servers using the `-s` option, or set up access as described in [setup for convenience](#setup-for-convenience).

## Usage for NTHUCAD
For NTHUCAD, since we have a set of servers, specify them in the file server `servers.txt` one address per line.


For checking for free GPUs on the server(s), simply add their address(es) after the script name.
You might not want to enter your password again and again, follow the steps in [setup for convenience](#setup-for-convenience).

```
> ./gpu_monitor.py myserver.com

Server myserver.com:
        GPU 0, GeForce GTX 1080
        GPU 1, GeForce GTX 1080 Ti
        ...
Sever myserver2.com: No free GPUs :(
```

If you have some set of servers that you regularily check, specify them in the file `servers.txt`, one address per line.
```
Once you did that, running just `./gpu_monitor.py` checks all servers specified in this file by default.

If you want to list all GPUs, utilization and who currently uses them, you can use the `-l` flag:
```
> ./gpu_monitor.py -l myserver.com

Server myserver.com:
        GPU 0 (GeForce RTX 2080 Ti  | gpu_util: 61 %  memory_util: 27 % ): Used by userA
        GPU 1 (GeForce RTX 2080 Ti  | gpu_util: 0 %   memory_util: 0 %  ): Used by userB
        ...
```

If you just want to see the GPUs used by yourself, you can use the `--me` flag.
This requires that your user name is the same as remotely, or that you specify the name using the `-s` flag.
```
> ./gpu_monitor.py --me myserver.com
Server myserver.com:
        GPU 1 (GeForce RTX 2080 Ti  | gpu_util: 61 %  memory_util: 27 % ): Used by userA
```

## Setup for Convenience

### Setting up an SSH key
If you want to avoid having to enter your password all the time, you can setup an SSH key to login into your server.
If you did this already, you are fine.

0. Log into NTHUCAD server
1. Open a terminal and run `ssh-keygen` and follow the instructions.
It might be a good idea to not use the default file but to specify a specific filename reflecting the servers you are connecting to.
3. Run `ssh-copy-id -i your_key_path icx`, where `icx` is the server you want to connect. 
        * Not necessary to repeat 3 since you have a shared home directory on all the servers like our server
5. Try to connect to the server using `ssh -x icx`.
The first time you connect, it should ask you for the password of the SSH key.
If you are asked for the password multiple times, you might need to manually activate your SSH key using `ssh-add <path_to_ssh_key>`.
If it still does not work, follow with the next steps.
