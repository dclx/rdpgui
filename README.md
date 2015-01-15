#!/usr/bin/env python
# -*- coding: utf-8 -*-

from PyQt4 import QtCore, QtGui
import os, time
import re
import subprocess
import urllib
import ConfigParser

abspath = os.path.abspath(__file__)
dname = os.path.dirname(abspath)
os.chdir(dname)

try:
    _fromUtf8 = QtCore.QString.fromUtf8
except AttributeError:
    def _fromUtf8(s):
        return s

try:
    _encoding = QtGui.QApplication.UnicodeUTF8
    def _translate(context, text, disambig):
        return QtGui.QApplication.translate(context, text, disambig, _encoding)
except AttributeError:
    def _translate(context, text, disambig):
        return QtGui.QApplication.translate(context, text, disambig)

class Ui_FULLGUI(object):
    def setupUi(self, FULLGUI):
        FULLGUI.setObjectName(_fromUtf8("FULLGUI"))
        FULLGUI.resize(1366, 768)
        icon = QtGui.QIcon()
        icon.addPixmap(QtGui.QPixmap(_fromUtf8("selectuser.png")), QtGui.QIcon.Normal, QtGui.QIcon.Off)
        FULLGUI.setWindowIcon(icon)
        FULLGUI.setStyleSheet(_fromUtf8("background-image: url(V8s.jpg);"))
        self.enterButton = QtGui.QPushButton(FULLGUI)
		self.enterButton.setGeometry(QtCore.QRect(970, 680, 157, 64))
        self.enterButton.setStyleSheet(_fromUtf8("background-color: rgb(255, 255, 255);"))
        self.enterButton.setText(_fromUtf8(""))
        icon1 = QtGui.QIcon()
        icon1.addPixmap(QtGui.QPixmap(_fromUtf8(login.png")), QtGui.QIcon.Normal, QtGui.QIcon.Off)
        self.enterButton.setIcon(icon1)
        self.enterButton.setIconSize(QtCore.QSize(157, 65))
        self.enterButton.setObjectName(_fromUtf8("enterButton"))

        self.retranslateUi(FULLGUI)
	centreWidget(FULLGUI)

    def retranslateUi(self, FULLGUI):
        FULLGUI.setWindowTitle(_translate("FULLGUI", "FULLGUI", None))
	self.enterButton.clicked.connect(self.handleButton)
	self.exitButton.clicked.connect(self.doExitNow)

    def doExitNow(self):
        sys.exit(app.exec_());

    def handleButton(self):
	commandline = str("python" + ' ' + "//usr//bin//rdpgui.py")
	#cleaning dquote from confi.ini params
	commandline = re.sub('["]','',commandline)
	#print commandline
	proc = subprocess.Popen(commandline, shell=True, stdout=subprocess.PIPE, stderr=subprocess.STDOUT)
	(out, err) = proc.communicate()
	print "-------------------------------------------------------------"
	print out
	if out.find("Authentication failure, check credentials") > 0:
		print "Authentication failure!"
	elif out.find("getaddrinfo (System error)") > 0 or out.find("getaddrinfo: System error") >= 0:
		print "Error connecting to server!"
	elif out.find("unable to connect to") >= 0 or out.find("A Remote Desktop Protocol client") >= 0:
		print "Error connecting to server!"
	else:
		print "Connecting to server!"


def centreWidget(self):
    screen = QtGui.QDesktopWidget().screenGeometry()
    mysize = self.geometry()
    hpos = ( screen.width() - mysize.width() ) / 2
    vpos = ( screen.height() - mysize.height() ) / 2
    self.move(hpos, vpos)


if __name__ == "__main__":
    import sys
    app = QtGui.QApplication(sys.argv)
    FULLGUI = QtGui.QWidget()
    ui = Ui_FULLGUI()
    ui.setupUi(FULLGUI)
    FULLGUI.setWindowFlags(QtCore.Qt.FramelessWindowHint)
    FULLGUI.show()
    FULLGUI.setFixedSize(FULLGUI.size());
    sys.exit(app.exec_())

