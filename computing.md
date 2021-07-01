

# Remote Computing

Zhentao Shi and Yishu Wang



To communicate with a remote server, two types of fundamental demands must be fulfilled: 

1. file upload and download: ftp/sftp, git; 
2. giving commands to the server: terminal. 



#### Useful Software/Apps

Firstly, you will need some tools to accomplish these tasks based on different operating systems (OS). Although you can use the original terminals provided by macOS and Linux to login onto a remote server by 

```
ssh userName@serverAddress
```

you will need to input your server address, password and initial commands every time. And this problem may be more annoying when you need to communicate with multiple servers. A more powerful terminal software can save you a lot of time in doing these tedious affairs by saving your keychains; also, it may allow you to save some frequently used commands or provide autocomplete functions. 

Here we recommend some software/apps to use based on our personal experience after function/cost balancing. 

| OS      | ftp/sftp            | terminal           |
| :------ | ------------------- | ------------------ |
| Window  | WinSCP              | PuTTY; MobaXterm   |
| macOS   | Royal TSX; Transmit | Royal TSX; Termius |
| iOS     | FE File Explorer    | Prompt; Termius    |
| Android | AndFTP              | JuiceSSH           |

iOS and Android apps are also listed for the following reasons. Firstly, nowadays Tablets become more and more powerful, especially working together with a keyboard. Some new tablets like iPad Pro (after 2020) and Huawei MatePad Pro could be more convenient to be used as a terminal device for remote servers than laptops. The advantages mostly lie in portability, battery endurance and cellular network, and thus you can work almost everywhere on a server as long as you have a tablets connected to 4G/5G network. Additionally, some time you will need to check on the progress of your program running on the server with you phone, say when having a meal outdoors, which also could be cool by the way! 



#### Git

An alternative solution to upload and download files on a server is to use git. In the terminal, you can do `clone`, `pull`, `add`, `commit`, `push`, and etc. See `Git.md` for more git commands. The most charming thing to use git rather than ftp/sftp is that the upload (push) and download (pull) are executed only on the server side. There are two benefits at least: 

1. It will cost you nearly zero network data usage, which is seriously important when using 4G/5G cellular network to work remotely. You do not want to think of uploading 1GB file to the server with ftp service using your data usage. 
2. The network stability during the transmission will be only depend on the server side, which helps a lot when your internet connection to the server is not stable. 



#### Useful Linux Commands

Most remote servers are build with Linux system without an graphic interface. To communicate with the server, we need to use Linux commands. Here we list some mostly used commands as following. 

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

Although you can use command `R`, `python`, `matlab`, `stata-mp` to open these software programs in the terminal, you may not like to use them in this way for the super inconvenience. Moreover, some software may provide a graphic interface by X-forwarding service, e.g. try `xstata-mp`. However, we do not encourage you to work in this way either, for the reason that you need to install X11 program on your local computer and the reaction is quite unstable. RStudio and Jupyter Notebook provide a better solution that you can use them within an internet browser. 

We strongly suggest you to write your code with your local editor, and then upload your code and data to the server to run your program remotely. Normally, for a Monte Carlo simulation program, we need to repeat the simulation for at least thousands of time, which may cost us a few minutes to several days. By running your program with no hang-up in the background, you can close your terminal to do something else while waiting for the results. For different software, the commands are as following. 

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

Here `nohup` stands for no hang-up, which allows you to close your terminal without stopping your program; and `&` stands for running in the background, which allows you to do something else within the terminal when your program is running. 

There are several important things should be noticed. 

1. `log.out` is the log file that all "print", "error" and etc. will be written in, and ideally it can be named by any text format. Do remember to write enough print-out messages in your code, especially when doing loops, thus you can check the progress of your program in the log file. Remember to often check for the progress. 
2. The current directory when you start to run your program is deemed as your working directory for your code. Please make sure that your "read" and "write" paths in the code are relatively based on this directory, or you can write a "change-working-directory" sentence at the beginning of the code. 
3. After starting your program in the background, use `top` to make sure that your program is running. 
4. Also, before running your program, use `top` to see other users' working status. Especially when doing parallel programming, make sure that you have enough computation power to meet your demand, and always remember to save some power for other users. 
5. Before the formal execution, run your code with few repetition times and small sample data to make sure that there is no bug in your code. You do not want to receive an error message after running the program for a very long time. 

Difficult in choosing programming languages? 

- 陈强-Stata, R与Python：我该选哪个语言-哔哩哔哩 https://b23.tv/tkAcHe



#### Scientific Computing Environment in the Department

There are three servers provided in our department. *EconSuper* is for teachers while *StudentHPC* is for RPG students, and both of them have 32-core CPU. The server addresses are listed as following and you can use RStudio and Jupyter Notebook in an internet browser. 

* *EconSuper*: `econsuper@econ.cuhk.edu.hk`

  RStudio: http://econsuper.econ.cuhk.edu.hk:8787/

* *StudentHPC*: `studenthpc@econ.cuhk.edu.hk`

  RStudio: http://studenthpc.econ.cuhk.edu.hk:8787/

* *SCRP*: `scrp-login.econ.cuhk.edu.hk`

  RStudio and Jupyter Notebook: http://scrp-login.econ.cuhk.edu.hk/

Originally, *EconSuper* and *StudentHPC* only provide R, Matlab and Stata, while *SCRP* also provides Python. You have authorities to install small software programs and packages under your own folder. For large software installation, please contact our technician. 

SCRP is the newest server in our department. For the information about SCRP, please see http://scrp.econ.cuhk.edu.hk. 

