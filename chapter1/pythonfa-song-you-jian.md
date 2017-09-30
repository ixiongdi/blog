```py
#!/usr/bin/env python
# coding=UTF-8
import smtplib

from email.mime.multipart import MIMEMultipart
from email.mime.text import MIMEText

import requests

data = {
    'username': 'xiongdi',
    'password': '*****',
    'backUrl': '/csv?tag=yangsen'
}

r = requests.post('http://uservalidation.i-md.com/login', data=data)

me = 'di.xiong@i-md.com'
you = 'di.xiong@i-md.com'

# Create message container - the correct MIME type is multipart/alternative.
msg = MIMEMultipart('alternative')
msg['Subject'] = 'Link'
msg['From'] = me
msg['To'] = you

# Create the body of the message (a plain-text and an HTML version).
text = u'这是yangsen的验证用户csv文件'

# Record the MIME types of both parts - text/plain and text/html.
part1 = MIMEText(text, 'plain', 'utf-8')

part2 = MIMEText(r.text, 'plain', 'utf-8')
part2['Content-Type'] = 'application/octet-stream'
part2['Content-Disposition'] = 'attachment; filename="out.csv"'

# Attach parts into message container.
# According to RFC 2046, the last part of a multipart message, in this case
# the HTML message, is best and preferred.
msg.attach(part1)
msg.attach(part2)

# Send the message via local SMTP server.
s = smtplib.SMTP('smtp.exmail.qq.com')
# sendmail function takes 3 arguments: sender's address, recipient's address
# and message to send - here it is sent as one string.
s.login('di.xiong@i-md.com', '****')
s.sendmail(me, you, msg.as_string())
s.quit()
```



