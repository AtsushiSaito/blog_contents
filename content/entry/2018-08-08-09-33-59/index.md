---
title: "【備忘録】PyQt5を用いたGUI Application開発"
date: 2018-08-08T09:33:59+09:00
categories:
tags:
keywords:
comments: false
showMeta: false
showActions: false
---

かなり前に使ったPyQt5を再度使おうとしたら殆ど忘れていたので, 記事に使い方をまとめていきたいと思います. 
内容は順次更新すると思うので, 内容が揃うまで時間が掛かるかもしれないです. 

## 動作環境

* Ubuntu 16.04
* たぶんROS Kineticに付いてきたPyQt5

## シンプルなウィンドウ
```python
from PyQt5.QtWidgets import *
import sys

class MainWindow(QWidget):
    def __init__(self):
        super(MainWindow, self).__init__()
        self.show()

if __name__ == '__main__':
    app = QApplication(sys.argv)
    main = MainWindow()
    sys.exit(app.exec_())
```


## ファイル選択ダイアログ
```python
from PyQt5.QtWidgets import *
import sys

class MainWindow(QWidget):
    def __init__(self):
        super(MainWindow, self).__init__()
        openButton = QPushButton('button', self)
        openButton.clicked.connect(self.showFileDialog)
        self.show()

    def showFileDialog(self):
        fname = QFileDialog.getOpenFileName(self, "Open file", "/home")
        print(fname)

if __name__ == '__main__':
    app = QApplication(sys.argv)
    main = MainWindow()
    sys.exit(app.exec_())
```

## matplotlibを埋め込んでみる
```python
from PyQt5.QtWidgets import *
from matplotlib.backends.backend_qt5agg import FigureCanvasQTAgg as FigureCanvas
import matplotlib.pyplot as plt
import numpy as np
import sys

class MainWindow(QWidget):
    def __init__(self):
        super(MainWindow, self).__init__()
        self.resize(500,500)
        self.openButton = QPushButton('Test Plot')
        self.openButton.clicked.connect(self.draw)

        # matplotlib
        self.figure = plt.figure()
        self.canvas = FigureCanvas(self.figure)

        # set the layout
        layout = QVBoxLayout()
        layout.addWidget(self.canvas)
        layout.addWidget(self.openButton)
        self.setLayout(layout)
        self.show()

    def draw(self):
        x = np.arange(-10, 10, 0.1)
        y = np.sin(x)
        ax = self.figure.add_subplot(111)
        ax.plot(x,y)
        self.canvas.draw()

if __name__ == '__main__':
    app = QApplication(sys.argv)
    main = MainWindow()
    sys.exit(app.exec_())
```