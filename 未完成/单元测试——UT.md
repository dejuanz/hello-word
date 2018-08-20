[TOC]

# 单元测试

**单元测试**是针对程序的最小单元来进行正确性检验的过程（一个单元可能是单个程序、类、对象、方法等）。

- 单元测试的优点：
  - 减少Bug
  - 快速定位Bug
  - 提高代码质量
  - 减少调试时间
- 单元测试缺点：
  - 周期时间长
  - 耗费资源
  - 能力要求高

## 单元测试流程

1. 单元测试——计划
   - 确定要测试的代码范围
     - 二八原则：频率、复用性、开发人员、复杂度
   - 评估标准——语句覆盖率、分支覆盖率、条件覆盖了、路径覆盖率、分支条件覆盖率

2. 测试策略——设计
   - 拿到代码进行调整——可独立执行
3. 测试策略——实现
   - 调整好的代码——画流程图
   - 流程图——流图——确定复杂度、路径
   - 根据复杂度和路径确定确定测试用例
4. 单元测试——执行
   - 使用unittestTestCase(测试框架)编写测试用例代码
   - 测试用例和测试数据分离
   - 生成测试报告 

## 单元测试策略

1. 自上向下

2. 自下向上

3. 孤立策略

   **注意：**当我们测试的代码有引用的其他函数时，需要打桩

## 代码与数据分离——参数化方式

1. XML格式
   - XML是一种标记语言，很类似HTML标记语言，XML是传输和存储数据，焦点在数据；HTML是显示数据，焦点在外观；
2. CSV格式
3. JSON串
4. TXT文本



### 总体流程

1. 创建Func、DataPool、ReadData、Cases、Reports、Tools文件夹
2. 在Func文件夹下存放要测试的代码
3. 在DataPool文件夹下存放数据。**注意：**不同格式的文件，数字格式不同
4. 在Cases文件夹下写测试用例
5. 在Reports里存放生成的html报告
6. 在Tool里存放HTMLTestRunner模板包
7. 另外创建一个文件写生成HTML报告的代码

### XML格式

#### Func 待测试代码

``` 
class Book(object):
    def book(self, a, b, c):
        if [a, b, c] == [1, 2, 3]:
            return "拇指姑娘"
        elif [a, b, c] == [4, 5, 6]:
            return "灰姑娘"
        elif [a, b, c] == [7, 8, 9]:
            return "国王的新衣"
        elif [a, b, c] == [10, 11, 12]:
            return "白雪公主"
```



#### XML格式数据

``` 
<?xml version="1.0" encoding="utf-8" ?>  # xml声明语句
<Book>
    <page>
        <page1>1</page1>
        <page2>2</page2>
        <page3>3</page3>
        <expect>拇指姑娘</expect>
    </page>
    <page>
        <page1>4</page1>
        <page2>5</page2>
        <page3>6</page3>
        <expect>灰姑娘</expect>
    </page>
    <page>
        <page1>7</page1>
        <page2>8</page2>
        <page3>9</page3>
        <expect>国王的新衣</expect>
    </page>
    <page>
        <page1>10</page1>
        <page2>11</page2>
        <page3>12</page3>
        <expect>白雪公主</expect>
    </page>
</Book>
```

#### ReadData

``` 
from xml.dom import minidom

class ReadXML(object):
    def readxml(self,node,i,child_node):
        dom = minidom.parse("../DataPool/data.xml")  # 加载解析  parse：从语法上描述或分析
        # print(dom)
        root = dom.documentElement  # 获取对象
        # elements = root.getElementsByTagName('page')
        #         # print(elements)  # 返回值为列表
        element = root.getElementsByTagName(node)[int(i)]  # 获取子元素
        data = element.getElementsByTagName(child_node)[0].firstChild.data  # 获取子元素值

        # print(element.getElementsByTagName('page1'))  # 返回值为只有1个元素的列表：[<DOM Element: page1 at 0x25757d2cc28>]
        return data

    def getLen(self,node):
        dom = minidom.parse("../DataPool/data.xml")  # 加载解析
        # print(dom)
        root = dom.documentElement  # 获取对象
        # elements = root.getElementsByTagName('page')
        #         # print(elements)  # 返回值为列表
        element = root.getElementsByTagName(node)
        return len(element)

```

#### Cases

``` 
import unittest
from Func.Book import Book
from ReadData.read_xml import ReadXML

book_class = Book()
read_class = ReadXML()


class TestXML(unittest.TestCase):
    def test_xml(self):
        for i in range(read_class.getLen('page')):
            data_list = [read_class.readxml('page', int(i), 'page1'),
                         read_class.readxml('page', int(i), 'page2'),
                         read_class.readxml('page', int(i), 'page3')]
            result = book_class.book(int(data_list[0]), int(data_list[1]), int(data_list[2]))
            self.assertEqual(result, read_class.readxml('page', int(i), 'expect'))

```



### JSON格式

#### Func 待测试代码

见xml中Func 待测试代码

#### JSON格式数据

``` 

  "data": [
    {
      "page1": 1,
      "page2": 2,
      "page3": 3,
      "expect":"拇指姑娘"
    },
    {
      "page1": 4,
      "page2": 5,
      "page3": 6,
      "expect":"灰姑娘"
    },
    {
      "page1": 7,
      "page2": 8,
      "page3": 9,
      "expect":"国王的新衣"
    },
    {
      "page1": 10,
      "page2": 11,
      "page3": 12,
      "expect":"白雪公主"
    }
  ]
}
```

#### ReadData

``` 
import json

class ReadJson(object):
    def readjson(self):
        with open('../DataPool/data.json','r',encoding="utf-8") as f:
            resource = json.load(f)
            # print(resource)  #{'data': [{'page2': 2, 'page1': 1, 'page3': 3}, {'page2': 5, 'page1': 4, 'page3': 6}, {'page2': 8, 'page1': 7, 'page3': 9}, {'page2': 11, 'page1': 10, 'page3': 12}]}
            data = resource["data"]
            # print(data)
            return data
```

#### Cases

``` 
import unittest
from Func.Book import Book
from ReadData.read_json import ReadJson

book_class = Book()
read_class = ReadJson()


class TestJson(unittest.TestCase):
    def test_json(self):
        for i in range(len(read_class.readjson())):
            data_list = [int(read_class.readjson()[i]["page1"]),
                         int(read_class.readjson()[i]["page2"]),
                         int(read_class.readjson()[i]["page3"])]
            result = book_class.book(data_list[0], data_list[1], data_list[2])
            self.assertEqual(result, read_class.readjson()[i]["expect"])
            print(result)
            print(int(read_class.readjson()[i]["page1"]),
                  int(read_class.readjson()[i]["page2"]),
                  int(read_class.readjson()[i]["page3"]),
                  read_class.readjson()[i]["expect"])
```



### CSV格式

#### Func 待测试代码

见xml中Func 待测试代码

#### CSV格式数据

``` 
1,2,3,拇指姑娘
4,5,6,灰姑娘
7,8,9,国王的新衣
10,11,12,白雪公主
```

#### ReadData

``` 
import csv

class ReadCSV(object):
    def readcsv(self):
        data_list = []
        with open("./DataPool/data.csv", 'r',encoding = 'utf-8') as f:
            lines = csv.reader(f)
            for i in lines:
                data_list.append(i)
            # print(data_list)
            return data_list
```

#### Cases

``` 
from ReadData.read_csv import ReadCSV
import unittest
from Func.Book import Book

read_book = Book()
readcsv = ReadCSV()


class TestReadCSV(unittest.TestCase):
    def test_read_csv(self):
        for i in range(len(ReadCSV().readcsv())):
            result = read_book.book(int(readcsv.readcsv()[i][0]),
                                    int(readcsv.readcsv()[i][1]),
                                    int(readcsv.readcsv()[i][2]))
            self.assertEqual(result, readcsv.readcsv()[i][3])

            print(int(readcsv.readcsv()[i][0]),
                  int(readcsv.readcsv()[i][1]),
                  int(readcsv.readcsv()[i][2]),
                  readcsv.readcsv()[i][3])

```

### TXT格式

#### Func 待测试代码

见xml中Func 待测试代码

#### TXT格式数据

``` 
1,2,3,拇指姑娘
4,5,6,灰姑娘
7,8,9,国王的新衣
10,11,12,白雪公主
```

#### Readdata

``` 
class ReadTXT(object):

    def readtxt(self):
        data_list = []
        with open("./DataPool/data.txt","r",encoding='utf-8') as f:
            data = f.readlines()
            # print(data)
            for i in data:
                i = i.strip().split(',')  #split:劈开、分离、分开  strip:除去、剥去
                data_list.append(i)
            # print(data_list)
            return data_list
```

#### Cases

``` 
from Func.Book import Book
from ReadData.read_txt import ReadTXT
import unittest

read_book = Book()
read_txt = ReadTXT()


class TestReadTXT(unittest.TestCase):
    def test_read_txt(self):
        for i in range(len(read_txt.readtxt())):
            result = read_book.book(int(read_txt.readtxt()[i][0]),
                                    int(read_txt.readtxt()[i][1]),
                                    int(read_txt.readtxt()[i][2]))
            self.assertEqual(result, read_txt.readtxt()[i][3])

            print(int(read_txt.readtxt()[i][0]),
                  int(read_txt.readtxt()[i][1]),
                  int(read_txt.readtxt()[i][2]),
                  read_txt.readtxt()[i][3])
```



### 生成测试报告

``` 
import unittest
import time

from Tools.HTMLTestRunner import HTMLTestRunner

# 需要测试的代码路径
test_dir = "./Cases/"
discover = unittest.defaultTestLoader.discover(test_dir, pattern="test_read_*.py")

if __name__ == '__main__':
    # 报告生成路径
    reports_dir = "./Reports/"
    # now time
    now_time = time.strftime("%Y%m%d-%H%M%S")
    # reports_name
    reports_name = reports_dir + now_time + 'reports.html'

    with open(reports_name, "wb") as f:
        # 实例化运行器
        runner = HTMLTestRunner(stream=f,verbosity=2,title="读书测试报告",description="win10" )
        runner.run(discover)

```



