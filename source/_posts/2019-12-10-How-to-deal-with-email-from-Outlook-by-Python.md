---
title: How to deal with email from Outlook by Python
date: 2019-12-10 21:53:24
tags:
 - Python自动化
 - 自动化测试
 - outlook
categories:
 - 自动化测试
 - Python自动化
 - Outlook
---

前面有篇文章介绍过如何使用Python的smtplib文件来操作POP3协议，以实现邮件收发的自动化操作，使用smtplib文件来收发邮件，需要事申请邮箱的授权码，并且在工作邮箱账户设置好授权码之后，脚本才能实现其功能。其实没有那么麻烦，这里在再介绍另外一种方法，可以更方便地实现邮件的自动化收发。

今天要介绍的就是Python的win32com接口文件，该文件几乎包含了所有的Windows API接口，是python编程必须掌握的核心模块，下面重点说说如何使用Win32模块实现Outlook的邮件收发功能。

1. 获取Win32模块 -
  + 通常通过python管道来安装 - pip install pywin32
  + 有时pip管道安装并不顺利，windows用户可以试着直接下载安装文件来直接安装：
  + [pywin32 module](https://sourceforge.net/projects/pywin32/files/pywin32)
2. 自定义类及收发邮件方法实现 -
3. 详细设计 -
4. 代码实现 -

<!--more-->

'''

    import win32com.client as win32
    class outlook():
      '''
      Operation local Outlook application
      '''
      def init(self):
        pass
      def openoutlook(self):
        pass
      def sendmail(self, receivers, title, body, attach_path=None):
        """
        Send the mail -
        ：param receivers：receivers
        ：param title：mail title
        ：param body：mail body
        ：param attach_path：attached file
        ；retun: send
        """
        outlook = win32.Dispatch('Outlook.Application')
        mail = outlook.CreateItem(0) # don't miss the initial parameter here

        # If multiple receivers, doing like this -
        if isinstance(receivers, list):
            if len(receivers) > 1:
                mail.To = ';'.join(receivers)
            else:
                mail.To = receivers[0]
        else:
            mail.To = receivers
        mail.Subject = title
        mail.Body = body
        if attach_path is None:
            pass
        else:
            mail.Attachments.Add(attach_path)
        mail.Send()


      def draftmail(self, receivers, title, body, attach_path=None):
        """
        Save the mail draft
        :param receivera:recievers
        :param title: title
        :param body: mail body
        :param attach_path: attached file
        :return:save
        """
        outlook = win32.Dispatch('Outlook.Application')
        mail = outlook.CreateItem(0)

        # If multiple receivers, doing like this -
        if isinstance(receivers, list):
            if len(receivers) > 1:
                mail.To = ';'.join(receivers)
            else:
                mail.To = receivers[0]
        else:
            mail.To = receivers
        mail.Subject = title
        mail.Body = body
        if attach_path is not None:
            mail.Attachments.Add(attach_path)
        mail.Save()

      def readNewMail(self):
        outlook = win32.Dispatch('Outlook.Application').GetNamespace("MAPI")
        inbox = outlook.GetDefaultFolder(6) # 6 means the inbox folder refer to https://docs.microsoft.com/en-us/office/vba/api/outlook.oldefaultfolders
        body_content = inbox.Items.GetLast().body
        return body_content

    if __name__ == '__main__':
        otlk = outlook()
        last_new_mail = otlk.readNewMail()
        print(last_new_mail)
        title = "Testing for mail sending automatically"
        receivers = "alan_yuan009@live.cn"
        body = 'This is a test mail which sended by Python script automatically, doing this, we hope we would use this to send our test bug regularlly, so dont reply it, it is test purpose only. Thanks. Alan!'
        filep = r"C:/RTKlog.log"
        otlk.sendmail(title=title, receivers=receivers, body=body, attach_path=filep)        
'''

**1. 另外，在安装pywin32模块时，要选择和python版本一致的安装包，下面几个常用python命令，可以查看Python的版本和详细安装信息-***

**DOS Mode -**
> - python
> + import sys
> * sys.version
> - sys.version_info
> + sys.winver

**2. pip管理第三方包常用命令**

    pip install python_moudule
    pip list
    pip install --upgrade somePackage
    pip uninstall somePackage
    freeze list

    python setup.py install (for the packages which unzip with setup.py file)
