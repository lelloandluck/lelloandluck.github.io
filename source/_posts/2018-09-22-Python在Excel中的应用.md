---
title: Python在Excel中的应用
date: 2017-09-22 21:45:41
tags:
 - python自动化
 - excel
categories:
 - python自动化
 - excel
---

这是之前做的一个python在excel中使用的例子，个人感觉还是蛮有意思的，今天拿出来分享给大家，其中也许有你感兴趣的地方。

1. 代码执行的结果如下 -
[查看结果](/images/Excution_Result.xlsx)

<!--More-->

2. 代码实现如下 -
```
from selenium import webdriver
from selenium.common.exceptions import NoSuchElementException
import time, os
from selenium.webdriver.common.keys import Keys
from selenium.webdriver.support.wait import WebDriverWait
from Python_TestReport_Excel_Template import *
from win32com.client import Dispatch, constants
import win32com.client
import os, time

def Preprocess_Environment_One_to_Five():
    # return 'startTime' - Don't change this return variable name because it will be used in report format function.
    os.system('taskkill /fi "imagename eq exce*" /f')
    os.system('taskkill /IM IEDriverSe* /f')
    os.system('taskkill /fi "imagename eq iexplo*" /f')
    initialTime = datetime.datetime.now()
    return initialTime

startTime = Preprocess_Environment_One_to_Five()


# Get current script path and folder
def CollectInfo_for_TestReport():
    # Get parameters of script path and report path
    getRootFolder = os.path.abspath(".") # get whole folder path which saved all current scripts.
    # get current Date and script Name
    getCurrentDate = time.strftime("%Y-%m-%d", time.localtime())
    suffixScriptName = os.path.basename(__file__)
    pureScriptName = suffixScriptName.split('.', 1)[0]
    reportPath = os.path.join(getRootFolder, "Test_Reports", getCurrentDate, pureScriptName)
    return pureScriptName, reportPath

pureScriptName, reportPath = CollectInfo_for_TestReport() # 这两个变量名不要更改名字，因为后面需要这两个变量作为参数值，因为名称完全一样，所以这里的变量名不要随意更改。


def Create_Report_and_Initialize_Headers(reportPath):
    # Create Report Path and report file and rename the default sheet name to Runtime_1
    reportFile = os.path.join(reportPath, "Excution_Result.xlsx")
    headersList = ('No', 'Checkpoints', 'Expectation Result', 'Real Result', 'Final Status', 'Screen Shot', 'Other Reference') # Header 请按此顺序定义column, 后续程序根据此column来展开脚本

    print ">>> Test report will be created and located here - ", reportFile

    if os.path.exists(reportPath): # 这里的参数不能为具体的文件名路径，只能是Folder的path
        pass
        print ">>> Report path was created already, please check the test report file and see if it also be created"
    else:
        os.makedirs(reportPath)  # 一次性创建多级目录，而mkdir一次只能创建一级目录, makedirs 和 mkdir只能创建文件夹，并不能穿件文件本身; 另外，makedirs不能覆盖存在的路径。
        # fo = open(reportFile, 'w+')
        # fo.close()  # 创建的Excel.xlsx文件没法打开，由于格式不兼容的问题
    if os.path.exists(reportFile):
        print ">>> Test report already created and here need to do is to add one more new sheet and initialized "
        xlApp = Dispatch("Excel.Application")  # 连接 excel 对象
        xlOpenWorkBook = xlApp.Workbooks.Open(reportFile)
        sheetCount = xlOpenWorkBook.Worksheets.Count
        addedSheetName = "Runtime_" + str(sheetCount + 1)
        xlApp.Worksheets.Add().Name = addedSheetName
        for i in range(0, len(headersList)):
            xlOpenWorkBook.Worksheets(addedSheetName).Cells(1, i+1).value = headersList[i]
        xlOpenWorkBook.Save() # 此处用Save()保存已打开的excel文件，无论之前的excel是否开启，都可以执行代码；不要用SaveAs(reportFile)会出现保存的文件已开启的提示框，而Python对windows下的excel提示框，不好操作
        xlApp.Quit()
    else:
        xlApp = Dispatch("Excel.Application")  # 连接 excel 对象
        xlBook = xlApp.Workbooks.Add()  # 新建一个excel文件
        xlBook.Worksheets[0].name = "Runtime_1"  # 默认情况下只创建包含一个名为 ‘sheet1’ 的表单
        for i in range(0, len(headersList)):
            xlBook.Worksheets[0].Cells(1, i+1).value = headersList[i] # initialize header base on the headerlist parameter
        xlBook.SaveAs(reportFile)
        xlApp.Quit()
        print ">>> Test Report was just created and initializated headers"
    return reportFile

    # define Report Headers -
# headerList = ('No', 'Checkpoints', 'Expectation Result', 'Real Result', 'Final Status', 'Screen Shot', 'Other Reference')
# reportFile = Create_Report_and_Initialize_Headers(reportPath, *headerList)
reportFile = Create_Report_and_Initialize_Headers(reportPath)


class Add_Checkpoints:
    # os.system('taskkill /fi "imagename eq exce*" /f')
    def byPassed(self, reportFile, checkPointStatement):
        xlApp = Dispatch("Excel.Application")
        xlOpenWorkBook = xlApp.Workbooks.Open(reportFile)
        sheetCount = xlOpenWorkBook.Worksheets.Count; objSheetName = "Runtime_" + str(sheetCount) #获得sheet name 主要是为了从sheet名称上来打开指定的sheet，当然也可以用sheets[0]来表示，因为在脚本中创建的sheet都是位于第一个sheet的位置
        rowCount = xlOpenWorkBook.Worksheets(objSheetName).UsedRange.Rows.Count

        objSheet = xlOpenWorkBook.Worksheets(objSheetName) # Notice 下面的写法同样有效。
        # objSheet = xlApp.Worksheets(objSheetName)
        objSheet.Cells(rowCount + 1, 1).Value = rowCount
        objSheet.Cells(rowCount + 1, 2).Value = checkPointStatement
        objSheet.Cells(rowCount + 1, 3).Value = "The checkpoint of '" + checkPointStatement + "' should be able to go smoothly with this pre-condition"
        objSheet.Cells(rowCount + 1, 4).Value = "Yeah ... ! Scripts really detected this checkpoint'" + checkPointStatement + "' at the specified condition"
        objSheet.Cells(rowCount + 1, 5).Value = "Pass"
        xlOpenWorkBook.Save() # Notice - Save 和Save()的区别，一个是单纯的宏命令动作，一个是方法，方法里面包含了对save这个动作的后续处理。所以这里如果只用Save则会弹出保存对话框，不如Save()方便直接。
        xlApp.Quit()

    def byFailure(self, reportFile, checkPointStatement):
        # os.system("taskkill /F /IM Exce*")
        xlApp = Dispatch("Excel.Application")
        xlOpenWorkBook = xlApp.Workbooks.Open(reportFile)
        sheetCount = xlOpenWorkBook.Worksheets.Count; objSheetName = "Runtime_" + str(sheetCount)  # 获得sheet name 主要是为了从sheet名称上来打开指定的sheet，当然也可以用sheets[0]来表示，因为在脚本中创建的sheet都是位于第一个sheet的位置
        rowCount = xlOpenWorkBook.Worksheets(objSheetName).UsedRange.Rows.Count

        objSheet = xlApp.Worksheets(objSheetName)
        objSheet.Cells(rowCount + 1, 1).Value = rowCount
        objSheet.Cells(rowCount + 1, 2).Value = checkPointStatement
        objSheet.Cells(rowCount + 1, 3).value = "The checkpoint of '" + checkPointStatement + "' should be able to go smoothly with this pre-condition" # please use plus symbol instead comma to combine varaible and string here
        objSheet.Cells(rowCount + 1, 4).Value = "Unfortunately ... ! Scripts didn't detect this checkpoint'"+ checkPointStatement +"' at the specified conditions" # 连接checkPointStatement变量，不能用comma,只能用plus/ + 号
        objSheet.Cells(rowCount + 1, 5).Value = "Failure"
        xlOpenWorkBook.Save()
        xlApp.Quit()


newClass = Add_Checkpoints()  # 必须要实例化类才能使用，这是常识。
# if 'xiaole' in "alan love xiaole":
#     newAClass.byPassed("Check string - 'alan' and see if it's existing in 'alan love xiaole' string")
# else:
#     newAClass.byFailure("Check string - 'alan' and see if it's existing in 'alan love xiaole' string")
#
# time.sleep(15)
for i in range(0, 1):
    if u"张寒冰" in u"这段文字中包含张寒冰三个字吗？":
        newClass.byPassed(reportFile, u"search '张寒冰' and see if it's searched in the gaven string")
    else:
        newClass.byFailure(reportFile, u"search '张寒冰' and see if it's searched in the gaven string")


    if u"张寒冰" in u"这段文字中能找出张汉滨吗？":
        newClass.byPassed(reportFile, u"search '张寒冰' and see if it's searched in the gaven string")
    else:
        newClass.byFailure(reportFile, u"search '张寒冰' and see if it's searched in the gaven string")

    if "morgan" in "www.morgan.com.\mainpage.index\Finace.us.123.pageo":
        newClass.byPassed(reportFile, "Check Mogstandley and see if it's in this string 'www.morgan.com'");
    else:
        newClass.byFailure(reportFile, "Check Mogstandley and see if it's in this string 'www.morgan.com'")

    if "Shanghai Financial" in "List all the records which search by Sublege for Baidu, and Tencent":
        newClass.byPassed(reportFile, "Check the string of 'subledge' and see if it will be show at the specified page which searched by definited for Baidu and Tencent")
    else:
        newClass.byFailure(reportFile, "Check the string of 'subledge' and see if it will be show at the specified page which searched by definited for Baidu and Tencent")

    if "Shanghai" in "www.morgan.com.mainpage.index.Shanghai Finanial Center":
        newClass.byPassed(reportFile, "Check Mogstandley and see if it's in this string 'www.morgan.com.\mainpage.index\Finace.us.233.pageContext'")
    else:
        newClass.byFailure(reportFile, "Check Mogstandley and see if it's in this string 'www.morgan.com.\mainpage.index\Finace.us.233.pageContext'")

    if "Xiaole" in "www.morgan.com\second1-2-3\content.list-us.sgn-Xiaole":
        newClass.byPassed(reportFile, "Check the string of 'Xiaole' and see if it will be show at the specified page content 'www.morgan.com\second1-2-3\content.list-us.sgn-Xiaole'")
    else:
        newClass.byFailure(reportFile, "Check the string of 'Xiaole' and see if it will be show at the specified page content 'www.morgan.com\second1-2-3\content.list-us.sgn-Xiaole'")

    if "LuckyLee" in u"请将每天的作业按时做完，然后也完成我的家庭作业和课外练习题，睡觉前送我检查并签字，切记！send to LucyLee":
        newClass.byPassed(reportFile, u"检查乐乐的作业情况是否按时完成！并记得每天签字 - Yuan, Jun-tao")
    else:
        newClass.byFailure(reportFile, u"检查乐乐的作业情况是否按时完成！并记得每天签字 - Yuan, Jun-tao")

# openedWorkBook = xlApp.Workbooks.Open("C:\\temp\\alan123.xlsx")
# print openedWorkBook.Worksheets.Count
# print openedWorkBook.Worksheets[0].name; openedWorkBook.Worksheets.Item(1).name # 两种写法均可获得sheet name
# print openedWorkBook.Worksheets[0].UsedRange.Rows.Count
# print openedWorkBook.Worksheets[0].UsedRange.Columns.Count
#`

def Format_Report(reportFile, scriptAuthor, startTime, getPureScriptName = pureScriptName):
    if os.path.exists(reportFile):
        # Step One - Create Excel.Application Object and Open an existing file and enter the specified sheet
        xlApp = Dispatch("Excel.Application")
        xlOpenWorkBook = xlApp.Workbooks.Open(reportFile)
        sheetCount = xlOpenWorkBook.Worksheets.Count;
        objSheetName = "Runtime_" + str(sheetCount)  # 获得sheet name 主要是为了从sheet名称上来打开指定的sheet，当然也可以用sheets[0]来表示，因为在脚本中创建的sheet都是位于第一个sheet的位置
        xlOpenSheet = xlOpenWorkBook.Worksheets(objSheetName)

        #Step Two - Format specified sheet Font.Size and Font Bold
        xlOpenSheet.UsedRange.Font.Name = "Gill Sans MT"
        xlOpenSheet.UsedRange.Font.Size = 10
        xlOpenSheet.UsedRange.Rows(1).Font.Size = 12 # Rows(1) 和 Row[1] 表示的意义不一样，前者代表第一行，后者表示第二行。这就是下标使用的作用
        xlOpenSheet.UsedRange.Rows(1).Font.Bold = True # Notice this expression and it's equal the usage below in Bold and Italic

        # Add border and add two more top row for Report Title
        xlOpenSheet.UsedRange.Borders.LineStyle = 1
        # xlOpenSheet.UsedRange.Rows(1).Borders.LineStyle = -4119
        xlOpenSheet.Rows[0].Insert()
        xlOpenSheet.Rows[0].RowHeight = 5 # 设置行高; ColumnWidth 用于设置列宽

        xlOpenSheet.Rows[0].Insert()
        xlOpenSheet.Rows[0].RowHeight = 30
        xlOpenSheet.Range("A1:G1").Borders(4).LineStyle = 1
        xlOpenSheet.Range("A1:G1").Borders(4).Weight = 3 # may be 4, 3, 1 not others

        xlOpenSheet.Range("A1:G1").Merge() # merge cells for report title text
        xlOpenSheet.Cells(1, 1).Value = "Execution Result for Scripts - " + pureScriptName # define the report title
        # xlOpenSheet.Range("A1").Font.ColorIndex = 5 # or 41 for setting  blue and light blue value
        xlOpenSheet.Cells(1, 1).Font.Name = "Gill Sans MT"; xlOpenSheet.Cells(1, 1).Font.Size = 15; xlOpenSheet.Cells(1, 1).Font.Bold = True

        # How to implement AutoFit?
        # xlOpenSheet.UsedRange.Columns.AutoFit()
        xlOpenSheet.UsedRange.EntireColumn.AutoFit()
        xlOpenSheet.UsedRange.HorizontalAlignment = 2 # maybe these value - Alignment, 1=auto | Alignment, 2=left | Alignment, 3=centre |Alignment, 4=right | |
        xlOpenSheet.UsedRange.VerticalAlignment = 2 # may be these initialization - Alignment, 1=top | Alignment, 2=middle | Alignment, 3=bottom |

        # Set report tile alignment and make it align on bottom
        xlOpenSheet.UsedRange.Rows(1).VerticalAlignment = 3

        # Format Result Color and make the failure record highlighted by Red font and then statistic the checkpoints result
        rowsCount = xlOpenSheet.UsedRange.Rows.Count; passQty = 0; failQty = 0

        for i in range(4, rowsCount + 1):
            if "Pass" == xlOpenSheet.Cells(i, "E").value: # Notice here - .value is quitely equal .Value
                passQty += 1
                pass
            else:
                print ">>> A failure checkpoint found"
                xlOpenSheet.UsedRange.Rows(i).Font.ColorIndex = 46 ; xlOpenSheet.UsedRange.Rows(i).Font.FontStyle = "Italic"; xlOpenSheet.UsedRange.Rows(i).Font.Bold = True  # ColorIndex value can refer to the "https://zhidao.baidu.com/question/90240687.html".
                xlOpenSheet.UsedRange.Cells(i, "E").Font.ColorIndex = 6 # set font to red
                # xlOpenSheet.UsedRange.Rows(i).Interior.ColorIndex = 3 #  set the selected block back graound color to red
                xlOpenSheet.UsedRange.Cells(i, "E").Interior.ColorIndex = 3 # set the cell back ground color not for Font color
                failQty += 1

        # 增加最左边一列，以利排版美观
        xlOpenSheet.Range("A:A").Insert() # Notice - this expression is actually equal with aove one on Insert()
        xlOpenSheet.Range("A1:A" + str(rowsCount + 7)).Borders(2).LineStyle = 1; xlOpenSheet.Range("A1:A" + str(rowsCount + 13)).Borders(2).Weight = 4 # 格式化首列的最右边的实线边框
        # xlOpenSheet.Cells(1, 1).Border(3).LineStyle = 1; xlOpenSheet.Cells(1, 1).Borders(3).Weight = 4; xlOpenSheet.Columns(1).ColumnWidth = 12 # Cells对象后面没有Borders属性，只能用Range来调用Border属性
        xlOpenSheet.Range("A1").Borders(4).LineStyle = 1; xlOpenSheet.Range("A1:A1").Borders(4).Weight = 3; xlOpenSheet.Columns("A").ColumnWidth = 4 # 格式化单元格A1的宽度和下划线, 注意这里Range("A1")不能写成Cells("A1"), 否则报错

        # 去掉视图的网格线以及标题栏和标尺等属性
        xlApp.ActiveWindow.DisplayGridlines = False
        xlApp.ActiveWindow.DisplayHeadings = False
        xlApp.ActiveWindow.DisplayFormulas = False
        xlApp.ActiveWindow.DisplayWhitespace = False
        xlApp.ActiveWindow.DisplayRuler = False

        # Extract and calculate Key words information
        xlOpenSheet.Cells(rowsCount + 3, 2).Value = "Key Information Extract -"; xlOpenSheet.Cells(rowsCount + 3, 2).Font.Name = "Gill Sans MT"; xlOpenSheet.Cells(rowsCount + 3, 2).Font.FontStyle = "Bold" ; xlOpenSheet.Range("B" + str(rowsCount + 3) + ":C" + str(rowsCount + 3)).Borders(4).LineStyle = 1 # 注意此处Bold的设置和前面同样效果的设置区别
        projectName = reportFile.split("\Test_Reports")[0].split("\\")[len(reportFile.split("\Test_Reports")[0].split("\\")) - 1]
        currentTime = datetime.datetime.now(); differ = (currentTime - startTime).seconds; h = differ / 3600; m = (differ % 3600) / 60; s = (differ % 3600) % 60 # the calculation difference between operate / and //, they would get the same value when the calculate under the all intgers, or difference
        durationTime = str(h)+':'+str(m)+':'+str(s)

        # Merge Rows and Definition the key words informations -
        xlOpenSheet.Range("B" + str(rowsCount + 4) + ":H" +  str(rowsCount + 12)).Merge()
        xlOpenSheet.Cells(rowsCount + 4, "B").Value = " Script Name: " + getPureScriptName + "     \n Project Name: " + projectName + "        \n Duration (Hr:Min:Sec) - " + durationTime + "       \n Passed Checkpoints: " + str(passQty) + "          \n Failed Checkpoints: " + str(failQty) + "        \n Automated Architecture Author: Alan.Yuan        \n Script Executor/Designer: " + scriptAuthor
        xlOpenSheet.UsedRange.Rows(rowsCount + 4).Font.Name = "Gill Sans MT"; xlOpenSheet.UsedRange.Rows(rowsCount + 4).Font.Size = 10 ; #xlOpenSheet.Range("B" + str(rowsCount + 4) + ":H" +  str(rowsCount + 12)).Font.FontStyle = "Italic";
        # xlOpenSheet.UsedRange.Rows(rowsCount + 4).VerticalAlignment = 1 # Notice - Range() 和 UsedRange.Rows()的用法，这里不可用Rows()取代UsedRange.Rows()，否则报错

        # rowCount = xlOpenWorkBook.Worksheets(objSheetName).UsedRange.Rows.Count
        xlOpenWorkBook.Save()

        print "So far, there are", xlOpenSheet.UsedRange.Rows.Count, "rows are available" # comma 连接的是任何数据类型，并且会自动加空格；plus连接的是同类型数据，不会自动加空格，这既是，和 + 连接符的区别
        print "So far, there are", xlOpenSheet.UsedRange.Columns.Count, "Columns are available"
        xlApp.Quit()

# scriptAuthor=u"Lello_Lucky"
Format_Report(reportFile, "Lello_Yuan", startTime)
```
