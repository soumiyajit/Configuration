1. Log in as a different user
2. Open the Terminal by pressing CTRL + ALT + T
3. Use the following commands:
   - sudo su -
   - vim /etc/gdm3/custom.conf
   - Add a new line and type Allowroot=True
   - Press i to edit, then esc, then type :wq
4. vim /etc/pam.d/gdm-password
   - Comment out the second line by using the hash # before the line
   - Press i to edit, then esc, then type :wq
5. passwd root
   - Set a new password for root
6. Log in as the root user on the Login Screen 
