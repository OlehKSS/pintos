# Pintos
This repo contains skeleton code for undergraduate Operating System course honor track at Peking University. 

[Pintos](http://pintos-os.org) is a teaching operating system for 32-bit x86, challenging but not overwhelming, small
but realistic enough to understand OS in depth (it can run on x86 machine and simulators 
including QEMU, Bochs and VMWare Player!). The main source code, documentation and assignments 
are developed by Ben Pfaff and others from Stanford (refer to its [LICENSE](./LICENSE)).

## Acknowledgement
This source code is adapted from professor ([Ryan Huang](huang@cs.jhu.edu)) at JHU, who also taught a similar undergraduate OS course. He made some changes to the original
Pintos labs (add lab0 and fix some bugs for MacOS). For students in PKU, please
download the release version skeleton code by `git clone git@github.com:PKU-OS/pintos.git`.

## How to run Pintos

```
podman build -t pintos .
```

Then run the docker image again but this time mount your `path/to/pintos` into the container.

*You need to enter the absolute path to your pintos directory in the command below!*

```
podman run -it --rm -v "$(pwd):/home/pintos" pintos
```
p.s. `--rm` tells docker to delete the container after running, and `--name pintos` names the container as `pintos`, this will be helpful in the debugging part.

This time when you `ls`, you will find there is a new directory called `pintos` under your home directory in the container.

Now here comes the exciting part:

```
cd pintos/src/threads/
make
cd build
pintos --
```

Bomb! The last command will trigger the qemu to simulate a 32-bit x86 machine and boot your Pintos on it. If you see something like

```
Pintos hda1
Loading............
Kernel command line:
Pintos booting with 3,968 kB RAM...
367 pages available in kernel pool.
367 pages available in user pool.
Calibrating timer...  32,716,800 loops/s.
Boot complete.
```

Your Pintos has been booted successfully, congratulations :)

You can close the qemu by `Ctrl+a+x` or `Ctrl+c` if the previous one does not work.

Now Let's conclude what you have done. First, You used docker to run a Ubuntu container which functions as a full Linux OS inside your host OS. Then you used qemu to simulate a 32-bit x86 computer inside your container. Finally, you boot a tiny toy OS -- Pintos on the computer which qemu simulates. Wow, virtualization is amazing, right?

Throughout this semester, you can modify your Pintos source code in your host machine with your favourite IDE and run/debug/test your Pintos in the container. You can leave the container running when you are modifying Pintos, and your modification will be visible in the container immediatedly.

## How to debug Pintos
If you have read the `Test and Debug` part in the Documentation, you may find that you need two terminals to debug the Pintos. This part teaches you how to do this in Docker.

First run the Pintos container as usual in one terminal following the command in the above section.

and run the commands below to boot Pintos in gdb mode.

```
cd pintos/src/threads/
pintos --gdb
```

Now open a new terminal and run 

```
docker exec -it <id> bash
```

You may remember in the above example you run your Pintos container and name it as `pintos`, then here you just exec a bash shell in your `pintos` container. 

Now you can run

```
cd pintos/src/threads/build
pintos-gdb kernel.o
```

and debug your Pintos as the Documentation says.

If you don't understand what these commands are doing, please read the Documentation first carefully.
