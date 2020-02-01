# cuckoo-download-instructions

Coming from my article on medium [Cuckoo Configuration](https://medium.com/@Newt_Tan/cuckoo-configuration-f40a395995c0)

This is the article about how to download and deploy the cuckoo for malware analysis. It took me around two months to figure it out(only with weekends of course). The host machine is win10 with ubuntu WSL and the guset machine is win7X64.

# Attention:
When you download the python, make sure it is v2X. Cuckoo can only be run with python2X. So when you download any libraries with pip, make sure the pip is version2.

The commands to check the version:
```
python --version
pip --version
```
So let’s go to the format context:

# Context:
Basically, you can refer to this link to finish the setting about the guest machine and your host machine. I will discuss more about the solutions of the problems I have met in the configuration process.

## Problems you may meet:

#### 1.About the WSL

(1)If you meet a problem when you start your WSL(ubuntu), the problem would be like register distribution error, try to uninstall all your anti-virus software, reboot your system after you uninstall the software. (Disable the firewall as well)
(2)Power shell should be executed with the administrator privilege (If os version is lower than 16000, there is no need to execute this command here).
```
Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux
```

#### 2. About the service

Basically, you can refer to this link to finish the setting about the guest machine and your host machine. Remember to replace using the mysql and apache server on WSL with XAMPP.
If you download the mysql and apache service before on your WSL, try to purge them first. Otherwise, the ports will have conflicts.
On your WSL:
```
sudo apt-get purge lamp-server^
```
Once you get the report, it means your configuration is right.

![cuckoo initializes right (without working directory)](https://github.com/Wapiti08/cuckoo-download-instructions/blob/master/Cuckoo/cuckoo_success.PNG)
You need to disable the firewall and automatic update.

![Disable the automatic update](https://github.com/Wapiti08/cuckoo-download-instructions/blob/master/Cuckoo/check_update.PNG)
Disable the version check in cuckoo cuckoo.conf
```
version_check = no
```
Do not comment on this line, otherwise, you will get an error.
Build a virtual environment:
```
virtualenv venv
activate    # use activate directly on 
```
activate the virtual environment
Create the working directory:

conflict of two cuckoo instances
Stop the cuckoo instance you have started before:

work in a directory

#### 3. Initialize the cuckoo web

I met a problem when I start the cuckoo web:
```
(base) C:\Python27\Scripts>cuckoo.exe web
c:\python27\python.exe: can't open file C:\Python27\Scripts\cuckoo': [Errno 2] No such file or directory
```

From this [link](https://github.com/cuckoosandbox/cuckoo/issues/1424), there is a solution by doing this:
> I copied the “cuckoo-script.py” from C:\Python27\Scripts to C:\Python27\Scripts and called it “cuckoo”.

But if you can not find the cuckoo-script.py ( For me, it is), try this [link](https://github.com/cuckoosandbox/cuckoo/issues/2439):
(Run as administrator)
```
> cd Scripts
> mklink cuckoo cuckoo.exe
```
Now, you can open your browser to type:
```
http://localhost:8000/
```
![Cheers!!!!](https://github.com/Wapiti08/cuckoo-download-instructions/blob/master/Cuckoo/local_success.png)


I prefer to submit examples through cuckoo api:
```
cuckoo submit (path of malware examples)
```
Please don't add --package after submit, it won't work.

#### 4. Issues:

(1) If your vms will shut off when you begin your cuckoo instance.

You can modify the mode = headless to mode = gui in the virtualbox.conf under conf folder. So the cuckoo won’t shut off the vms any more!!!

(2) If you meet problem about the permission of tcpdump(renamed Windump). You need to modify two places in sniffer.py (The location is easy to find with tool named everything).

```
for line in err.split(“\r\n”):
if not line continue or line.startswith(err_whitelist_start):
continue
```

```
err_whitelist_start = (
“tcpdump: listening on “,
“/mnt/c/bin/tcpdump.exe: listening on “,
```
##### NOTE: The path should reflect the actual path to where you installed tcpdump.exe.

Here is an example on WSL. If you download winpdump on your Win system, please modify it to your own path.

After you modify the path of tcpdump.exe. You need to add the same path to your auxiliary.conf under .cuckoo.

(3) If you meet the error: 
```
cuckoo is running. PID:xxx
```
On Windows command prompt:
```
taskkill /F /PID 8712(PID number)
```



The following links you may need when you download the pip, Microsoft Visual C++ Compiler, etc:

https://www.gungorbudak.com/blog/2018/08/02/correct-installation-and-configuration-of-pip2-and-pip3/

https://stackoverflow.com/questions/43645519/microsoft-visual-c-9-0-is-required

https://www.microsoft.com/en-us/download/details.aspx?id=44266

# Reference:
https://docs.cuckoosandbox.org/en/latest/installation/guest/network/

https://www.youtube.com/watch?v=nLGJHgv6uWA




