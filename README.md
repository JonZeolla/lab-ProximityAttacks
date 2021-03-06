# Proximity Attacks Lab

## How to clone this branch
* `git clone --recursive https://github.com/JonZeolla/lab-ProximityAttacks`
  * Clone the latest revision of the lab-ProximityAttacks repo and automatically bring in the related submodules.

## Related Events
**2016-08-11 Steel City Information Security Lab**

http://www.meetup.com/Steel-City-InfoSec/events/227075388/

## How to setup the lab
### Configure the lab manually  
#### Linux
From the top level of the source code repository run:  
`./setup.sh`

#### Windows
From the top level of the source code repository, right click the `setup.ps1` file and click "Run with PowerShell".  These scripts are not currently signed, so you may need to set `Set-ExecutionPolicy` properly.  

### Use the provided Virtual Machine
A VM will be provided for the lab (TODO).  Only **VMWare hypervisors** have been tested with the following configuration.  
##### Login Credentials
* Username:  prox
* Password:  P@ssword

### Configure a new VM for distribution
Note:  This setup will only handle the RFID stations.  Because of Windows licensing and the fact that my MSR605 only works on Windows, I am unable to distribute that as a VM.  

1. Install Kali 2016.1 in a VM
2. Login as root, then open a terminal
3. (optional) Take a snapshot
4. Run the following commands:

    ```
    apt-get -y update && apt-get -y upgrade
    apt-get -y install open-vm-tools-desktop fuse
    history -c && gnome-session-quit
    ```
5. VM Tools should now be active, allowing you to easily copy and paste into the VM.  Login as root again, then open a terminal and run the following commands:

    ```
    echo -e "# Add additional sbin and bin directories to \$PATH\nexport PATH=\$PATH:\${HOME}/bin:/sbin:/usr/sbin:/usr/local/sbin:/opt/devkitpro/devkitARM/bin/\n\n# Include .bashrc if it exists\nif [ -f "\${HOME}/.bashrc" ]; then\n  . "\${HOME}/.bashrc"\nfi\n" > /etc/skel/.bash_profile
    echo -e "# Kali rolling repos\ndeb http://http.kali.org/kali kali-rolling main contrib non-free\n#deb-src http://http.kali.org/kali kali-rolling main contrib non-free" > /etc/apt/sources.list
    echo -e "\n\n# Add additional sbin and bin directories to \$PATH\nexport PATH=\$PATH:\${HOME}/bin:/sbin:/usr/sbin:/usr/local/sbin:/opt/devkitpro/devkitARM/bin/\n" >> /etc/skel/.profile
    echo "pXloRpmKEasnWPCUihcQcx1WeUo9fo2hQJAXh1uoAOQ1ooz3xLUCbPYDItfeULA9zItnZaQqfell0LLBzSuQhxl98dyP8y7DY1hE" > /etc/scis.conf
    useradd -m -p $(openssl passwd -1 P@ssword) -s /bin/bash -c "SCIS Proximity Attacks User" -G sudo prox
    history -c && gnome-session-quit
    ```
6. Login as prox and rearrange the Favorites shortcuts as appropriate
7. Open a terminal and run the following commands:

    ```
    sudo systemctl disable rsyslog;sudo systemctl stop rsyslog
    sudo logrotate -f /etc/logrotate.conf
    sudo rm -rf /var/log/*1 /var/log/*old /var/log/*gz
    cd ${HOME}/Desktop
    git clone -b ProximityAttacks --single-branch --recursive https://github.com/JonZeolla/lab
    lab/setup.sh
    exitcode=$?
    sudo systemctl enable rsyslog
    rm ~/.bash_history
    if [[ ${exitcode} == 0 ]]; then history -c && shutdown -P now; fi
    ```
8. Create the OVA. On a Mac using VMware Fusion, this looks something like:

    ```
    cd /Applications/VMware\ Fusion.app/Contents/Library/VMware\ OVF\ Tool/
    ./ovftool --acceptAllEulas /path/to/VM.vmx /path/to/VM.ova
    ```

## Updating this branch  
### Linux
If you'd like to update this branch, open a terminal and `cd` into the repo (if you are following the lab, this is `${HOME}/Desktop/lab/`) and then run:  

```
git pull
./setup.sh
```
 * It is possible that you will need to first run `git reset --mixed`, depending on if the `git merge` can be successful without manual intervention.  Note that running this command will reset your index, but not the working tree.  If you don't know what that means, and would like to, read [this](https://git-scm.com/docs/git-reset).  

### Windows
There is no current way to update this branch on a Windows machine.  

## Some other good materials
### General Hardware / Software
* https://www.kickstarter.com/projects/rysccorp/proxmark-pro-proxmark3-without-a-pc
* http://news.mit.edu/2016/hack-proof-rfid-chips-0203
* http://shop.riftrecon.com/products/rfidler
* https://github.com/linklayer/BLEKey
* https://store.ryscc.com/products/new-proxmark3-kit
* http://www.bishopfox.com/resources/tools/rfid-hacking/attack-tools/
* http://www.boscloner.com/build-instructions.html
* https://silent-pocket.com/
* http://rfidguardian.org/index.php/Main_Page
* https://github.com/emsec/ChameleonMini
* https://github.com/coldfusion39/VertXploit
* https://github.com/AdamLaurie/RFIDIOt
* https://github.com/ApertureLabsLtd/RFIDler
* https://github.com/Proxmark/proxmark3
* https://github.com/samyk/magspoof
* https://pmwdzvbyvnmwobk5.onion.link/project/freakcard
* https://github.com/brad-anton/proxbrute
* https://github.com/brad-anton/VertX
* https://github.com/exploitagency/github-proxmark3-standalone-lf-emulator

### Android
* https://developer.android.com/guide/topics/connectivity/nfc/hce.html
* https://sourceforge.net/projects/nfcproxy/

### Apple iOS
* https://support.apple.com/en-us/HT203027

### Misc
* http://www.radio-electronics.com/info/wireless/nfc/near-field-communications-modulation-rf-signal-interface.php
* http://docs.lib.purdue.edu/cgi/viewcontent.cgi?article=1033&context=techmasters
* https://www.youtube.com/watch?v=1Xz5HgOL_Gc&list=PLDIb0pk-F3fXXsU90h3WZS9uxyj1rMW0E
* https://www.youtube.com/watch?v=Bttr7fEfxiE ([slides](https://www.blackhat.com/presentations/bh-dc-08/Franken/Presentation/bh-dc-08-franken.pdf))
* https://www.youtube.com/watch?v=seKas8KFcSI
* http://proxclone.com/

## Contributing
1. [Fork the repository](https://github.com/jonzeolla/lab-ProximityAttacks/fork)
1. Create a feature branch via `git checkout -b feature/description`
1. Make your changes
1. Commit your changes via `git commit -am 'Summarize the changes here'`
1. Create a new pull request ([how-to](https://help.github.com/articles/creating-a-pull-request/))

