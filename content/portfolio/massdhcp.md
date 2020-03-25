+++
title = "Installing MAAS and Configuring DHCP"
slug = "mass-dchp"
date = "2019-04-06"
+++



{{< figure src="/portfolio/static/maas.png" height="300">}}


MAAS is a tool for provisioning baremetal servers in the cloud. The newest release of MAAS, 2.6.0, introduces a new way to configure DHCP. This tutorial will walk you through installing MAAS on Ubuntu 18.04, and configuring DHCP through the web interface. 


## Installing MAAS


### Configuring Ubuntu 

1. Update your system

        sudo apt update && sudo apt upgrade
2. Create a user, add the user to the `sudo` group, log in to the newly created user: 
        
        sudo adduser user 
        sudo adduser user sudo
        su user

### Installing MAAS

1. Install version 2.6.0 of MAAS, by adding the `maas/next` ppa:

        sudo apt-add-repository ppa:maas/next

2. After adding the PPA, press `ENTER` after reading the following prompt: 
  
    {{< highlight conf >}}
 This PPA holds new upstream development releases, usually development releases. Currently, it is holding the 2.5 series.
 More info: https://launchpad.net/~maas/+archive/ubuntu/next
Press [ENTER] to continue or Ctrl-c to cancel adding it.
{{</ highlight>}}

3. Update the package list:

        sudo apt update
    
4. Install MAAS 2.6.0: 

        sudo apt install maas

5. Initialize MAAS: 
  
        sudo maas init

6. Follow the prompt to create an admin user: 

        Create first admin account
        Username: maas-admin
        Password:
        Again:
        Email: angel@staghunt.org
        Import SSH keys [] (lp:user-id or gh:user-id):


### Start the MAAS server

If MAAS was successfully installed, the web interface will be available at: `http://<IPADDRESS>:5240`

1. Navigate to the MAAS web interface in your browser, you will be greeted by this screen: 

  ![loginscreen](/portfolio/static/loginscreen.png)

    Enter the username and password you created in Step 6, of the Installing MAAS section. 
    
2. Verify that all of the information is correct in both of the following screens:

  ![Screen2](/portfolio/static/screen2.png)
  ![screen3](/portfolio/static/screen3.png)
  
3. Add an SSH key to your account. If you use Launchpad, you can import your key using your Launchpad username. If you need to create an SSH key, run the following command, in the terminal: 

        ssh-keygen -t rsa
    
    Follow the prompts, your SSH key will be saved at `/home/user/.ssh/id_rsa.pub`. 
    
4. Use the `cat` command to display the key in the terminal. Using your mouse manually copy the key and paste it into the MAAS interface. 

        cat /home/user/.ssh/id_rsa.pub

5. Click the **Import** button, and after confirming that the interface looks like this: 

  ![ssh-key](/portfolio/static/ssh-key.png)

Press the **Return to Dashboard** button. 


## Configuring DHCP 
