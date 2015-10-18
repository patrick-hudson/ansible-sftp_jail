# Ansible script to automatically create a jailed SFTP user.

1. It configures sshd
2. Sets the sftp subystem and match user block
3. Creates a group called sftponly 
4. Assigns a user of your choice to that group 
5. Generates a random password and hash for said user
6. Creates a folder in /home/ for them
7. Creates a sitename folder to bind mount to
8. Bind mounts and configures /etc/fstab with the correct values.
9. Restarts SSHD

How to use

```ansible-playbook --extra-vars "username=myusername mountroot=/var/www/vhosts/mysite.com" -i hosts site.yml```

2 simple parameters to pass.

* Username
* Folder you want to mount to the users home directory

After it runs, your output should look something like this

```~/jail_sftp_ansible# ansible-playbook --extra-vars "username=patrick mountroot=/var/www/vhosts/whotheip.com" -i hosts site.yml

PLAY [test-playbook | Test sftp-server role] **********************************

GATHERING FACTS ***************************************************************
ok: [127.0.0.1]

TASK: [jail_sftp | Create sftp user group] ************************************
ok: [127.0.0.1]

TASK: [jail_sftp | Alter sftp subsystem entry] ********************************
ok: [127.0.0.1]

TASK: [jail_sftp | Test for Match Block] **************************************
ok: [127.0.0.1]

TASK: [jail_sftp | Get sshd_config content] ***********************************
ok: [127.0.0.1]

TASK: [jail_sftp | Apply sshd_config template] ********************************
ok: [127.0.0.1]

TASK: [jail_sftp | Generate password] *****************************************
changed: [127.0.0.1]

TASK: [jail_sftp | Hash password] *********************************************
changed: [127.0.0.1]

TASK: [jail_sftp | Create sftp users] *****************************************
changed: [127.0.0.1]

TASK: [jail_sftp | Correct ownership and permission of home directories] ******
ok: [127.0.0.1]

TASK: [jail_sftp | Get Site Name] *********************************************
changed: [127.0.0.1]

TASK: [jail_sftp | Create Mount Folder] ***************************************
ok: [127.0.0.1]

TASK: [jail_sftp | Bind Mount] ************************************************
ok: [127.0.0.1]

TASK: [jail_sftp | Print users password] **************************************
ok: [127.0.0.1] => {
    "msg": "Finished! Your new SFTP Jailed account has been created. Username myuser Password  R5+J&rqcfh(A[7cM"
}

PLAY RECAP ********************************************************************
127.0.0.1                  : ok=14   changed=4    unreachable=0    failed=0

```

### Pay mind to the last message. That's your users username and password.

