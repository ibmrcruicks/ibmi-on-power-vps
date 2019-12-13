# ibmi-on-power-vps
setting up an IBMi instance on Power Virtual Server instance in IBM Cloud

NB: you will need to have a Pay-As-You-Go or Subscription account on IBM Cloud
to be able to set up an IBMi (or AIX) virtual server.

The first in the journey requires a "Power Systems Virtual Server" service instance,
which you wiill find through the `Catalog` menu.

Search for "Power Systems Virtual" and you should find one entry:
![PSVS tile](res/img/psvs-tile.png).

Click on that tile, and you will be presented with a service creation form, and have the option to 
select where you want your IBMi instance(s) to be created.

Current (Dec 2019) choices are:
+ Dallas
+ Washington DC
+ Frankfurt

Frankfurt appears to have larger capacity available, so pick that region/location,
unless you need your system to be in the USA.

![PSVS setup](res/img/psvs-setup.png)

Click `Create` and a new service instance will be established for you.

## ssh keys

## terminal emulation
when your VPS LPAR is active, you will be able to launch access to the virtual console through the web emulator.
![console](res/img/psvs-console.png)

note that the emulator supports Function Key emulation with a virtual keypad at the bottom of the window - 
actually, this keypad is initially hidden below the bottom of the window -- you will need to stretch the window to 
make the keypad visible. This is necessary for any function keys higher than 12, whether your keyboard has
physical function keys or not.
![display no_keypad](res/img/psvs-emulator.png)  
![display 1_keypad](res/img/psvs-emul-keypad1.png)  
![display 2_keypad](res/img/psvs-emul-keypad2.png)  

## first login

+ QSECOFR/QSECOFR -- change the password (max length is defaulted to 8 - you can update this later by running
  ```
  CHGSYSVAL QPWDMAXLEN VALUE('16')
  ```
  for whatever length of password you want to accommodate.
+ **accept all the software licence agreements (20 or more) -- each group must be viewed and accepted individually**
+ create a user profile for regular login instead of QSECOFR (if this user is set up with *SECOFR authority,
  remember to set 
  ```
  CHGSYSVAL QLMTSECOFR VALUE('0')
  ```
  to allow login through 5250 terminal sessions
+ make sure the SSH daemon is running
  ```
  STRTCPSVR SERVER(*SSHD)
  ```
+ you may also want to enable telnet for 5250 sessions
  ```
  STRTCPSVR SERVER(*TELNET)
  ```
+ update the DST login as the password will be set to expired, using the supplied documentation - 
  [config docs](https://cloud.ibm.com/docs/infrastructure/power-iaas?topic=power-iaas-configuring-ibmi)

