

# Remote Computing

Zhentao Shi and Yishu Wang



To communicate with a remote server, two types of jobs must be fulfilled: 

1. file upload and download: ftp/sftp, git; 
2. execute commands on the server: terminal. 



#### Useful Software/Apps

On different operating systems (OS) we need software or apps to facilitate these tasks. Although in principle we can rely the original terminals provided by macOS and Linux to log into a remote server by 

```
ssh userName@serverAddress
```

We must manually  input the server IP address, password and the initial commands every time. This problem may be more annoying when we communicate with multiple servers. A modern powerful terminal software can save time for these tedious chores by storing your keychains and providing autocompletion.

Based on our personal experience and function/cost balancing, we recommend the following software/apps. 

| OS      | ftp/sftp            | terminal           |
| :------ | ------------------- | ------------------ |
| Window  | WinSCP              | PuTTY; MobaXterm   |
| macOS   | Royal TSX; Transmit | Royal TSX; Termius |
| iOS     | FE File Explorer    | Prompt; Termius    |
| Android | AndFTP              | JuiceSSH           |

iOS and Android apps are listed for the following reasons. Firstly, nowadays Tablets become more and more powerful, especially working together with a keyboard. As a terminal device for remote servers, new tablets like iPad Pro (after 2020) and Huawei MatePad Pro are more convenient than laptops. The advantages mostly lie in portability, battery endurance and cellular network, and thus we can work almost everywhere on a server as long as you have a tablets connected to 4G/5G network. Additionally, some time you will need to check on the progress of your program running on the server with you phone, say when having a meal outdoors, which also could be cool by the way!



#### Git

An alternative solution to upload and download files on a server is to use git. In the terminal, you can do `clone`, `pull`, `add`, `commit`, `push`, and etc. See `Git.md` in this repository for more git commands. The most charming thing to use git rather than ftp/sftp is that the upload (push) and download (pull) are executed on the server side only. There are two benefits at least: 

1. It will cost nearly zero network data usage, which is seriously important when using 4G/5G cellular network to work remotely. We want to avoid uploading 1GB file to the server with a ftp service via our mobile data plan. 
2. The network stability during the transmission will be solely depend on the server side, which helps tremulously when the internet connection to the server is unstable. 



#### Linux Commands

Most remote servers are built upon the Linux system without an graphic interface. To communicate with the server, we count on the Linux commands. Here we list some most useful commands as following. 

- Login to the server: 

  ```
  ssh userName@serverAddress
  ```

- Change directory: 

  ```
  cd directory
  ```

  1. `directory` can be an absolute directory or relative directory with respect to the current location; 
  2. `*`: the current directory; 
  3. `..`: the parent directory; 
  4. `~`: your user's root directory. 

- List files under the current folder: 

  ```
  ls
  ```

- Create a new folder: 

  ```
  mkdir -p dirName
  ```

- Copy: 

  ```
  cp -r oldDir newDir
  ```

- Move or rename: 

  ```
  mv oldDir newDir
  ```

- Delete: 

  ```
  rm -rf fileName
  ```

- Check file size: 

  ```
  du -sh fileName
  ```

- Check the process: 

  ```
  top
  ```

- Terminate a process: 

  ```
  kill PID
  pkill -9 processName
  ```

  1. You can check `PID` with `top`; 
  2. `pkill` is more convenient for stopping parallel computing as you can kill all the processes of a program at a same time. 

- Check the command history: 

  ```
  history
  ```

  1. You can also type the "up" button to go through the recent commands; 
  2. `history -c`: clear all the command history. 



#### Run Program

The commands  `R`, `python`, `matlab`, `stata-mp` can open these programs in the terminal interactively, but the user interface is unfriendly. Although some software offers a graphic interface by X-forwarding service, e.g. try `xstata-mp`. However, we do not encourage in this way either, as the X11 program must be installed on your local computer and the reaction is quite unstable. RStudio and Jupyter Notebook provide a better solution via an internet browser. 

We recommend writing the code in a local editor, and then upload the code and data to the server to execute remotely. Normally, for a Monte Carlo simulation program, we need to repeat the simulation for thousands of times, which may cost a few minutes to several days. By running the program with no hang-up in the background, it is safe to close the terminal while waiting for the results. For different software, the commands are as following. 

- R: 

  ```
  nohup Rscript fileName.R > log.out &
  ```

- Python: 

  ```
  nohup python fileName.py > log.out &
  ```

- Matlab: 

  ```
  nohup matlab -nodesktop -nodisplay fileName.m > log.out &
  ```

- Stata: 

  ```
  nohup stata fileName.do > log.out &
  ```

Here `nohup` stands for no hang-up, which allows us to close the terminal without terminating the program; and `&` stands for running in the background, which allows us to do something else within the terminal instead of waiting until the execution is complete. 

There are several important things to noticed. 

1. `log.out` is the log file that all "print", "error" and etc. will be written in, and ideally it can be named by any text format. Do remember to write enough print-out messages in your code, especially when doing loops, and thus we can check the progress of the program in the log file. Remember to check the progress. 
2. The current directory when we start to run our program is taken as the working directory. Please make sure that the "read" and "write" paths in the code are *relatively* to this directory. Otherwise, we write a "change-working-directory" sentence at the beginning of the code. 
3. After starting some programs in the background, use `top` to make sure that they are in the process. 
4. Also, before running programs, use `top` to see other users' working status. This is important especially when doing parallel programming. Make sure that we have enough computation resources to meet our demand, and always remember to spare some resources for other users. 
5. Before the formal execution, run the code with few repetitions and small sample data to make sure that there is no bug in your code. No one want to receive an error message after running the program for a very long time. 





#### Scientific Computing Environment in the Department

There are three servers provided in our department. *EconSuper* is for teachers while *StudentHPC* is for RPG students, and both of them have 32-core CPU. The server addresses are listed as following and you can use RStudio and Jupyter Notebook in an internet browser. 

* *EconSuper*: `econsuper@econ.cuhk.edu.hk`

  RStudio: http://econsuper.econ.cuhk.edu.hk:8787/

* *StudentHPC*: `studenthpc@econ.cuhk.edu.hk`

  RStudio: http://studenthpc.econ.cuhk.edu.hk:8787/

* *SCRP*: `scrp-login.econ.cuhk.edu.hk`

  RStudio and Jupyter Notebook: http://scrp-login.econ.cuhk.edu.hk/

Originally, *EconSuper* and *StudentHPC* only provide R, Matlab and Stata, while *SCRP* also provides Python. One has permission to install small software programs and packages under one's own designated folder. For large software installation, please contact our technicians in our department. 

SCRP is the newest server in our department. For the information about SCRP, please see http://scrp.econ.cuhk.edu.hk. 

