##Secure access to linux server for ubuntu 18-20

This procedure is for setting up secure access to a linux computer or VPS
It assumes you have already set up a VPS at a service like digital ocean or OVH
It assumes you have been given credentials for SSH access to the VPS, and that you know how to do that
it also assumes that once its installed, you know how to run it.
Once you have gained root access you may follow these steps to 1) set up a secure server and then 2) set up a Divi wallet


#First update linux

    sudo apt update    (depending on your provider, you may or may not need sudo)
    sudo apt upgrade
    sudo apt reboot

#After reconnecting set up a user and grant the root privillidges

    adduser johndoe (follow along with setting up password, use numbers and letters only! dont worry about address or room number)
    usermod -a -G sudo johndoe

#Now change the SSH connection characteristics

    sudo nano /etc/ssh/sshd_config
    Change: 
        Port 22 , change port to XXXX (something greater than 1024)
        PermitRootLogin to no
        PasswordAuthentication yes
        AllowUsers johndoe  

    sudo service ssh reload
    sudo systemctl restart ssh

    Now in a separate connection, login ssh -p XXXX johndoe@ipaddress
    Verify this connection works
    THEN close root login window (you will not be able to use this connection again)

#Now logged in as your super user (johndoe in this case),

    sudo apt install fail2ban   (this will prevent login after 2 failed attempts)
    sometimes to get it to work:
        sudo apt-get remove fail2ban
        sudo apt-get autoremove
        sudo rm -rf /etc/fail2ban/
        sudo apt-get install fail2ban

#Set up swap file, this will use some hard drive space for memory not accessed often
    
    sudo fallocate -l 2G /newswapfile
    sudo chmod 600 /newswapfile
    sudo mkswap /newswapfile
    sudo swapon /newswapfile

    Make swap permanent
        echo '/newswapfile none swap sw 0 0' | sudo tee -a /etc/fstab

    see Swappiness
        cat /proc/sys/vm/swappiness

#At this point you have a secure VPS that you can access with username and password
#The rest is about how to install a Divi node. You can probably easily substitute other blockchain nodes

#Install unzip
    sudo apt install unzip
         
    copy link from https://github.com/DiviProject/Divi/releases/  
    depends on which is the latest version, it should be in the form: divi-x.x.x-x86_64-linux-gnu-xxxxxx.tar.gz

#Install Divi
    wget https://github.com/DiviProject/Divi/releases/xxx/divi-x.x.x-x86_64-linux-gnu-xxxxxx.tar.gz
    tar -xvf divi-x.x.x-x86_64-linux-gnu-xxxxxx.tar.gz
    



