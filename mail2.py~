import getpass
from email.MIMEText import MIMEText
from smtplib import SMTP
from smtplib import SMTP_SSL


# from poolhouse.tools.logger import log
class Log(object):
    @classmethod
    def debug(cls, string):
        print string

    @classmethod
    def error(cls, string):
        print string


def send_email_alert(content, content_type, subject, sender_email,
                     sender_password, sender_smtp_server, sender_port,
                     reciever_email, reciever_smtp_server,
                     receiver_port, use_ssl, use_tls):
    """
    This function will send an email from the specified user to another
    specified user. No smtp servers are assumed. SSL/TLS is supported.
    All information needed must be specified.
    """

    try:
        # Setup Message
        msg = MIMEText(content, content_type)
        msg['Subject'] = subject
        msg['From'] = sender_email

        # Connect to SMTP Server
        if use_ssl:
            conn = SMTP_SSL(sender_smtp_server, sender_port)
            Log.debug("Established connection with SMTP server")
        else:
            conn = SMTP(sender_smtp_server, sender_port)
            Log.debug("Established connection with SMTP server")
        if use_tls:
            conn.ehlo()
            conn.starttls()
            conn.ehlo()
        conn.set_debuglevel(0)  # show communication with server if True
        conn.login(sender_email, sender_password)
        Log.debug("Logged in")

        # Send Email
        try:
            conn.sendmail(sender_email, reciever_email, msg.as_string())
            Log.debug("Sent mail")
        finally:
            conn.close()
    except Exception, exc:
        Log.error("Failed to send mail.")
        Log.error("{}".format(exc))


def main():
    content = 'Test message'
    content_type = 'plain'
    subject = "Sent from Python"

    sender_email = raw_input('Please specify sender email ID: ')
    sender_password = getpass.getpass(
        'Enter Password for {}: '.format(sender_email))
    sender_smtp_server = 'smtp.gmail.com'
    sender_port = int(raw_input('Please specify the port: '))
    reciever_email = raw_input('Please specify receiver email ID: ')
    use_ssl = int(raw_input('Enter 1 if SSL is enabled, else enter 0: '))
    use_tls = int(raw_input('Enter 1 if TLS is enabled, else enter 0: '))
    reciever_smtp_server = None
    receiver_port = None

    send_email_alert(content, content_type, subject, sender_email,
                     sender_password, sender_smtp_server, sender_port,
                     reciever_email, reciever_smtp_server,
                     receiver_port, use_ssl, use_tls)

if __name__ == "__main__":
    main()
