# sendmail-linux
# https://medium.com/@yenthanh/config-your-sever-as-a-mta-mail-transfer-agent-using-sendmail-with-a-gmail-account-93bbf2eec6c1
# how to configure sendmail to send from gmail accounts

#Config your sever as a MTA (Mail Transfer Agent) using ‘sendmail’ with a gmail account 


Today I need to setup a system that auto send alert email when anything go wrong using NetData. This framework use ‘sendmail’ command to send alert emails. So I need to setup the sendmail command in my server for NetData to use it whenever it need to send alert email.

There are many choice to config sendmail, I can use some good service like SendGrid,… but it is not cost-effective. So I decided to create a new gmail account and config sendmail with it.

#Install sendmail and config
First, install the sendmail:

`$ sudo apt-get install sendmail`

Note: Make sure to configure gmail to access less secure app from https://admin.google.com/ --> basic security
