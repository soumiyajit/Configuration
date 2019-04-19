## TigerVNC configuration for CentOS7

#### Step1: Install TigerVNC from your terminal.
yum install tigervnc-server

#### Step2: Login to the user you want VNC program and configure the VNC password:
su - your_user<br/>
vncpasswd

#### Step3: Add the VNC conf file for the user. For my server I have the root privileges.
cp /lib/systemd/system/vncserver@.service  /etc/systemd/system/vncserver@:1.service

#### Step4: Copy the following configurations to the vncserver template copied from /etc/systemd/system/ directory and replace the values to reflect your user as shown in the below sample.
vi /etc/systemd/system/vncserver@\:1.service

Add the following lines to the file vncserver@:1.service.<br/>

[Unit]<br/>
Description=Remote desktop service (VNC)<br/>
After=syslog.target network.target<br/>

[Service]<br/>
Type=forking<br/>
ExecStartPre=/bin/sh -c '/usr/bin/vncserver -kill %i > /dev/null 2>&1 || :'<br/>
ExecStart=/sbin/runuser -l my_user -c "/usr/bin/vncserver %i -geometry 1280x1024"<br/>
PIDFile=/home/my_user/.vnc/%H%i.pid<br/>
ExecStop=/bin/sh -c '/usr/bin/vncserver -kill %i > /dev/null 2>&1 || :'<br/>

[Install]<br/>
WantedBy=multi-user.target<br/>

#### Step5: Now we should restart the daemon and start the vnc server:

systemctl daemon-reload<br/>
systemctl start vncserver@:1<br/>
systemctl status vncserver@:1<br/>
systemctl enable vncserver@:1<br/>

Check if the status of the vncserver is active.

Output of "systemctl status vncserver@:1":<br/>
● vncserver@:1.service - Remote desktop service (VNC)<br/>
   Loaded: loaded (/etc/systemd/system/vncserver@:1.service; disabled; vendor preset: disabled)<br/>
   Active: active (running) since Fri 2019-04-19 12:25:24 IST; 7s ago<br/>
  Process: 5598 ExecStart=/sbin/runuser -l root -c /usr/bin/vncserver %i -geometry 1280x1024 (code=exited, status=0/SUCCESS)<br/>
  Process: 5593 ExecStartPre=/bin/sh -c /usr/bin/vncserver -kill %i > /dev/null 2>&1 || : (code=exited, status=0/SUCCESS)<br/>
 Main PID: 5623 (Xvnc)<br/>
   CGroup: /system.slice/system-vncserver.slice/vncserver@:1.service<br/>
           ‣ 5623 /usr/bin/Xvnc :1 -auth /root/.Xauthority -desktop madrid:1 (root) -fp c...<br/>

Apr 19 12:25:18 madrid systemd[1]: Starting Remote desktop service (VNC)...<br/>
Apr 19 12:25:24 madrid systemd[1]: Started Remote desktop service (VNC).<br/>


#### Step6: Check if the ports are in listening state by using the following command:
ss -tulpn| grep vnc<br/>

tcp    LISTEN     0      5         *:5901                  *:*                   users:(("Xvnc",pid=5623,fd=9))<br/>
tcp    LISTEN     0      128       *:6001                  *:*                   users:(("Xvnc",pid=5623,fd=6))<br/>
tcp    LISTEN     0      5        :::5901                 :::*                   users:(("Xvnc",pid=5623,fd=10))<br/>
tcp    LISTEN     0      128      :::6001                 :::*                   users:(("Xvnc",pid=5623,fd=5))<br/>

#### Step7: Open the vnc ports to allow passthrough firewall using the following command:
firewall-cmd --add-port=5901/tcp<br/>
firewall-cmd --add-port=5901/tcp --permanent<br/>

#### Step8: We are pretty done on the server side. Now to configure the client, we need to install the vncviewer on the client OS which should be pretty easy and straigh forward. Refer the following link for the vncviewer installation guide:
https://www.realvnc.com/en/connect/download/viewer/

