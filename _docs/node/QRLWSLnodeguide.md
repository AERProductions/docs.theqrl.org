---
title: QRL WSL Node full guide
categories: node
description: The QRL Node documentation
tags: node
---

Running a QRL node strengthens the network, supports the decentralization and further verifies transactions on the network. This is an essential function of the decentralized architecture QRL relies on.

This allows you to run a private secure node to communicate with the QRL blockchain. 

You can use the node to connect the explorer, wallet, and ephemeral messaging features to the gRPC QRL functions. 

> There are various options available for connecting to the API and setup options for the node can be configured through a user set configuration file.
{: .info}


## Requirements

You can run QRL on most operating systems, though Ubuntu 20.04 is recommended.
{: .info}

- Support for AES-NI
- Support for avx2 (Used by keccak library for hashing functions)
- HDD with enough storage for the blockchain as it grows
- Reliable network connection 
- Python3.6
- 64 bit processor


#### Ubuntu on Linux Subsystem for Windows (WSL)

WSL FAQ (Useful info, if you need it): https://docs.microsoft.com/en-us/windows/wsl/faq

WSL 2 is only available in Windows 10:
For x64 systems: Version 1903 or higher, with Build 18362 or higher.
For ARM64 systems: Version 2004 or higher, with Build 19041 or higher.
Builds lower than 18362 do not support WSL 2 and use WSL1 instead.

To update to WSL 2, you must be running Windows 10. Otherwise, you must use WSL 1.
Check your Windows version by selecting the Windows logo key + R, type winver, select OK.
(Or enter the ver command in Windows Command Prompt).
https://docs.microsoft.com/en-us/windows/wsl/compare-versions
WSL1 has better cross-OS file system performance and doesn't require VM (WSL1 is the better choice for projects that need tools on both OS as it has better file system access)
WSL2 has Managed VM, Virtual Call and Full Linux Kernel among other benefits but lacks in some ways that WSL1 does not.


## Install QRL 

1. Install WSL ( https://docs.microsoft.com/en-us/windows/wsl/install-manual#step-6---install-your-linux-distribution-of-choice )

Run powershell as administrator and run this command (you can copy and paste):
```
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
```

For WSL 1, we recommend now moving on to step #3.
To update to WSL 2, wait to restart your machine and move on to step #2.


2. Install VM by running this command in powershell (ran as admin)
```
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```

2a. To continue installing WSL 2 move to step 10a, otherwise move to step 3.

3. Download Ubuntu

https://aka.ms/wslubuntu2004

This will cause the <distro>.appx packages to download to a folder of your choosing. 
You should rename it to Ubuntu, so that it goes from this: Ubuntu_2004.2020.424.0_x64.appx

to this: Ubuntu.appx

4. Install Ubuntu

Once the distribution has been downloaded, navigate to the folder containing the download and run the following command in that directory, where app-name is the name of the Linux distribution .appx file.

So run the command: ```Add-AppxPackage .\Ubuntu.appx```

If you are using Windows server, or run into problems running the command above you can find the alternate install instructions on the Windows Server documentation page to install the .appx file by changing it to a zip file. https://docs.microsoft.com/en-us/windows/wsl/install-on-server

4a. Create User account for Ubuntu

After it installed (it might appear to do nothing), I simply hit enter and then it asked me to create a User account with pass.

5.Add your Linux distribution path to the Windows environment PATH (C:\Users\Administrator\Documents\Ubuntu in this example), using PowerShell:

```
$userenv = [System.Environment]::GetEnvironmentVariable("Path", "User")
[System.Environment]::SetEnvironmentVariable("PATH", $userenv + ";C:\Users\Administrator\Documents\Ubuntu", "User")
```

6. Update and upgrade Ubuntu with this command:

```
sudo apt update && sudo apt upgrade -y
```

7. Install all required dependencies:

```
sudo apt-get -y install swig3.0 python3-dev python3-pip build-essential pkg-config libssl-dev libffi-dev libhwloc-dev libboost-dev cmake
```

8. Install setupTools

```
pip3 install -U setuptools
```

9. Install QRL

```
pip3 install -U qrl
```

10a. To finish installing WSL 2:

After installing VM in step 2, Restart your machine to complete the WSL install and update to WSL 2. Download the WSL 2 linux kernel here: https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi
Run the update package downloaded in the previous step. (Double-click to run - you will be prompted for elevated permissions, select ‘yes’ to approve this installation.)

Once the installation is complete, move on to the next step - setting WSL 2 as your default version when installing new Linux distributions. (Skip this step if you want your new Linux installs to be set to WSL 1).

10b. Set WSL 2 as your default version
Open PowerShell and run this command to set WSL 2 as the default version when installing a new Linux distribution:

```
wsl --set-default-version 2
```

10c. Select 'Get' for linux distro on windows store, use this link (It's Ubuntu 20.04):
https://www.microsoft.com/store/apps/9n6svws3rx71
The first time you launch a newly installed Linux distribution, a console window will open and you'll be asked to wait for a minute or two for files to de-compress and be stored on your PC. All future launches should take less than a second.

You will then need to create a user account and password for your new Linux distribution.
Proceed to step 4a.

## Start QRL Node

If you haven't restarted yet, do so now and then resume from this point

10. Run QRL

start_qrl

If all went well, it should run and sync, it will create a data folder in 

C:\Users\Administrator\Documents\Ubuntu\rootfs\home\*YourUbuntuUserName*\.qrl

Stop it with ctrl+c and navigate your browser to this website: https://qrl.co.in/chain/#mainnet-chain

Download the mainnet chain state file here:
https://cdn.qrl.co.in/mainnet/QRL_Mainnet_State.tar.gz

After it downloads (It is almost 12gb, give it time) you'll need 7zip or similar so download it here if you don't have it already: 
https://www.7-zip.org/download.html

Navigate your explorer file browser to C:\Users\Administrator\Documents\Ubuntu\rootfs\home\*YourUbuntuUserName*\.qrl\data\state

Utilize 7zip to open the .tar.gz which will reveal another .tar inside, extract that folder to the QRL directory > data folder where the other state folder is: C:\Users\Administrator\Documents\Ubuntu\rootfs\home\*YourUbuntuUserName*\.qrl\data

Replace any files it asks.

Your blockchain files should now be closely synced with the LIVE chain, saving you more than a weeks worth of syncing time. These chain files are provided by a QRL team member, so they should be safe. If you prefer, you can sync the entire file.

Now go here 
https://docs.theqrl.org/node/configuration/

and find this link near the top
https://docs.theqrl.org/node/config.yml

right click it and save content as, save this config file into the .qrl directory:

You'll have to edit it with notepad or notepad++ ( https://github.com/notepad-plus-plus/notepad-plus-plus/releases/download/v8.1.9/npp.8.1.9.Installer.exe ), so open it with either one and edit these lines, removing the # from infront of them and adding your mining wallet address:

mining_enabled: Enabled
mining_address: 'Your Wallet Address'
mining_thread_count: 0  # 0 to auto detect thread count based on CPU/GPU number of processors

If you don't have a wallet, you can generate one with the mobile app, the desktop wallet ( https://p2pq.trade/viewforum.php?f=37 ), or the web wallet ( https://p2pq.trade/viewforum.php?f=23 ).

If you have permissions issues, if you do, run this command:

```
sudo chmod -R a+rwx \.qrl\
```

Finally, run start_qrl and it should work!
You should now have a syncing, mining QRL node!

#### Help

If you have any issues, carefully read through this tutorial again and all of the source information and then contact #support on the discord ( https://discord.com/invite/RcR9WzX )!

If you would like to see all of the options you can pass along the command line simply add `--help` to the end of the command above.

```bash
{{ layout.v.qrlCommands.startQRL }} --help
```

This will print all of the various options available. 

```bash
usage: start_qrl [-h] [--mining_thread_count MINING_THREAD_COUNT] [--quiet]
                 [--qrldir QRL_DIR] [--no-colors]
                 [-l {DEBUG,INFO,WARNING,ERROR,CRITICAL}]
                 [--network-type {mainnet,testnet}]
                 [--miningAddress MINING_ADDRESS]
                 [--mockGetMeasurement MEASUREMENT] [--debug] [--mocknet]

QRL node

optional arguments:
  -h, --help            show this help message and exit
  --mining_thread_count MINING_THREAD_COUNT, -m MINING_THREAD_COUNT
                        Number of threads for mining
  --quiet, -q           Avoid writing data to the console
  --qrldir QRL_DIR, -d QRL_DIR
                        Use a different directory for node data/configuration
  --no-colors           Disables color output
  -l {DEBUG,INFO,WARNING,ERROR,CRITICAL}, --loglevel {DEBUG,INFO,WARNING,ERROR,CRITICAL}
                        Set the logging level
  --network-type {mainnet,testnet}
                        Runs QRL Testnet Node
  --miningAddress MINING_ADDRESS
                        QRL Wallet address on which mining reward has to be
                        credited.
  --mockGetMeasurement MEASUREMENT
                        Warning: Only for integration test, to mock
                        get_measurement
  --debug               Enables fault handler
  --mocknet             Enables default mocknet settings
```


{: .info}