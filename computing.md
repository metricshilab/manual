

# Remote Computing

Zhentao Shi and Yishu Wang



To communicate with a remote server, two types of fundamental demands must be fulfilled: 

1. file upload and download: ftp/sftp, git; 
2. giving commands to the server: terminal. 



#### Useful Software/Apps

Firstly, you will need some tools to accomplish these tasks based on different operating systems (OS). Although you can use the original terminals provided by macOS and Linux to login onto a remote server by 

```
ssh UserName@ServerAddress
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
2. The network stability during the transmission will be only depend on the server side, which helps a lot when the university VPN is not stable. 



#### Useful Linux Commands

Most remote servers are build with Linux system without an graphic interface. To communicate with the server, we need to use Linux commands. 

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
  pkill -9 processName
  ```

- Check the command history: 

  ```
  history
  ```

  1. You can also type the "up" button to go through the recent commands. 
  2. `history -c`: clear all the command history. 



#### Run Program

Although you can use command `R`, `python`, `matlab`, `stata-mp` to open these software programs in the terminal, you may not like to use them in this way for the inconvenience. 



#### Scientific Computing Environment in the Department

* EconSuper
* StudentHPC
* SCRP



陈强-Stata, R与Python：我该选哪个语言-哔哩哔哩 https://b23.tv/tkAcHe

