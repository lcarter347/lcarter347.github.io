---
layout: post
title:  "Resetting passwords"
date:   2016-03-26 12:00:23 +0000
categories: buymybooks
---

# Resetting passwords

I decided that I wanted to implement a forgot password functionality for the site. I debated between setting up security questions or sending 
an email with a new password. Ultimately I opted for the latter, because I was interested in whether I could even do this with python.


I found out that python has an smtplib that allows you to send email, but in order to send it you need to have an smtp server to send it from. 
After a bit of research, it seemed like using gmail’s smtp server would be the easiest way. It didn’t work at first. But then I found out that 
in order to use their server, you have to supply your login and password and everything has to be encrypted using SSL. So I created a gmail account 
for BuyMyBooks, and added a few lines of code to establish an SSL connection. 


Once I did that it worked. However, the to, from, and subject lines weren’t working properly (they were all jumbled together in the from line). 
I looked into it, and found out that python supports working with mimetext. First, I had to import MIMEText from email.mime.text. Next, I created 
a mimetext object, and gave it a body and to, from, and subject values. Then all I had to do was convert the mimetext object to a string and send it using smtplib.  

Here is the code for this (Note: ****** stands in for the BuyMyBooks gmail password, which I had to hard code in):
```
receiver=[request.form['username']]
sender = ['buymybooks350@gmail.com']

emailMSG = "Your new password is:  " + randomPass + "\n\nThank you, \nBuyMyBooks"
msg = MIMEText(emailMSG)
msg['Subject'] = 'Reset password'
msg['From'] = 'buymybooks350@gmail.com'
msg['To'] = request.form['username']

try:
    smtpObj = smtplib.SMTP("smtp.gmail.com", 587)
    #server.set_debuglevel(1)
    smtpObj.ehlo()
    smtpObj.starttls()
    smtpObj.login('buymybooks350@gmail.com', '******')
    smtpObj.sendmail(sender, receiver, msg.as_string())         
    print "Successfully sent email"
except Exception as e:
    print(e)

```

The only other thing I really needed to figure out after that was how to generate random passwords in Python. This turned out to be fairly easy. Python 
has a random.choice() function that can be called on a list or string and which will pick one item from that list/string at random. In my code, I called this function, 
and passed it all of the uppercase and lowercase ascii letters and the numbers 0-9 (which can be referred to through the Python constants string.ascii_uppercase, string.ascii_lowercase, 
and string.digits). Then I just nested this code in a loop (which I chose to call 8 times, to create a password 8 characters long) and joined these individual choices together into 1 string. 


All of this is accomplished in just a couple of lines of code:

```
chars = string.ascii_uppercase + string.ascii_lowercase + string.digits
randomPass = ''.join(random.choice(chars) for _ in range(8))
```


After that, I just had to write the code to make the necessary updates in the database, and I was good to go.
