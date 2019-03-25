# sendmail-linux
# https://medium.com/@yenthanh/config-your-sever-as-a-mta-mail-transfer-agent-using-sendmail-with-a-gmail-account-93bbf2eec6c1
# how to configure sendmail to send from gmail accounts

#Config your sever as a MTA (Mail Transfer Agent) using ‘sendmail’ with a gmail account 


Today I need to setup a system that auto send alert email when anything go wrong using NetData. This framework use ‘sendmail’ command to send alert emails. So I need to setup the sendmail command in my server for NetData to use it whenever it need to send alert email.

There are many choice to config sendmail, I can use some good service like SendGrid,… but it is not cost-effective. So I decided to create a new gmail account and config sendmail with it.

#Install sendmail and config
First, install the sendmail:

`$ sudo apt-get install sendmail`
Edit file /etc/hosts and make sure there is a line as below, this step is needed to help command ‘sendmailconfig’ run faster

`127.0.0.1       localhost localhost.localdomain <your hostname>`
Config sendmail with below command and press “Y” whenever requested:

`sendmailconfig`

#Create your gmail account and add it into sendmail
Now, just go ahead and create your gmail account, remember the password as we need it for configuration.

Now we add the new gmail account into our server. Create a new directory:

`mkdir -m 700 /etc/mail/authinfo/
cd /etc/mail/authinfo/`

In the new directory, create a file named gmail-auth and input your gmail account here (don’t worry about your password, we will delete this file in the next steps):

`AuthInfo: "U:root" "I:YOUR GMAIL ADDRESS" "P:YOUR GMAIL PASSWORD"`
Create a hash for ‘gmail-auth’

`makemap hash gmail-auth < gmail-auth`

You can see a new file is created with name ‘gmail-auth.db’, we actually use this file for authentication with your gmail account so you can delete file ‘gmail-auth’ now

`rm /etc/mail/authinfo/gmail-auth`


Now we open file /etc/mail/sendmail.mc and add those lines, right above first “MAILER” definition line, DON’T PUT THE LINES ON TOP OF THE ‘sendmail.mc’ file

```define(`SMART_HOST',`[smtp.gmail.com]')dnl```
```define(`RELAY_MAILER_ARGS', `TCP $h 587')dnl```
```define(`ESMTP_MAILER_ARGS', `TCP $h 587')dnl```
```define(`confAUTH_OPTIONS', `A p')dnl```
```TRUST_AUTH_MECH(`EXTERNAL DIGEST-MD5 CRAM-MD5 LOGIN PLAIN')dnl```
```define(`confAUTH_MECHANISMS', `EXTERNAL GSSAPI DIGEST-MD5 CRAM-MD5 LOGIN PLAIN')dnl```
```FEATURE(`authinfo',`hash -o /etc/mail/authinfo/gmail-auth.db')dnl```

Finally, let’s build sendmail again and restart it

`make -C /etc/mail
/etc/init.d/sendmail reload`

#Test sending email with ‘sendmail’ command
Now everything is done. Let’s try to send your first email to your ‘target-email@gmail.com’ with command:

`/usr/sbin/sendmail -t <<EOF
To: target-email@gmail.com
Subject: my subject
Content-Type: text/html
hello world
EOF`






Note: Make sure to configure gmail to access less secure app from https://admin.google.com/ --> basic security
