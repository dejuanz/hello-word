[TOC]

# Fixture

fixture是一个概述，对一个测试用例环境的初始化和销毁就是一个Fixture.

- 对象级别
- 类级别
- 模块级别

**Module：**在unittest框架下，一个py文件就是一个测试用例，在有必要情况下，可以定义一个模块，在当前测试用例的开始和结束时运行

``` 
import unittest

def setUpModule():  # 定义fixture模块控制级别的初始化：在整个模块的最开始执行
    print("setupmodule")

def tearDownModule():  # 定义fixture模块控制级别的销毁：在整个模块结束时执行
    print("teardownmodule")

class Fixture(unittest.TestCase):
    @classmethod
    def setUpClass(cls):  # 定义fixture类控制级别的初始化：在类开始时执行
        print("setupclass")

    @classmethod
    def tearDownClass(cls):  # 定义fixture类控制级别的销毁：在类结束时执行
        print("teardownclass")

    def setUp(self):  # 定义fixture方法级别的初始化：有多少以“test"开头的对象方法，就执行几次
        print("setup")

    def tearDown(self):   # 定义fixture方法级别的销毁：有多少以“test"开头的对象方法，就执行几次
        print("teardown")

    def test_fixture(self):
        print("test_fixture")
```

# Skip

**skip：**对于 一些未完成的或者不满足条件的测试函数和测试类，可以跳过执行。

``` 
import unittest

version = 3.5


class demo_test(unittest.TestCase):
    def test_skip1(self):
        print("demo_1")

    @unittest.skip
    def test_skip2(self):
        print("demo_2")

    @unittest.skipIf(version > 2.0, "version>2.0时跳过")
    def test_skip3(self):
        print("demo_3")

@unittest.skipIf("version>3.0","version>3.0时跳过")
class demo_SkipIf(unittest.TestCase):
    def test_skip4(self):
        print("demo_4")

    @unittest.skip
    def test_skip5(self):
        print("demo_5")

    @unittest.skipIf(version > 2.0, "version>2.0时跳过")
    def test_skip6(self):
        print("demo_6")
```



# Parameterize

**parameterize:**unittest本身不支持参数化，需要下载一个parameterized插件，利用parameterized.expand()实现参数化

``` 
import unittest
from parameterized import parameterized  # unittest测试框架中本身不支持参数化，需要安装parameterize控件


def build_data():
    return [(1, 2, 3), (1, 0, 1), (0, 0, 1)]


def add(x, y):
    return x + y

class TestPara(unittest.TestCase):
    @parameterized.expand(build_data)  # 也可以直接将列表传入
    def test_para(self,x,y,expect):
        result = add(x,y)
        self.assertEqual(result,expect)
```



# 方法封装PO模式

## PO模式分层

PO模式分为三层：对象库层、操作层、业务层

- 对象库层：封装定位元素的方法
- 操作层：封装对元素的操作
- 业务层：将操作组合起来完成一个业务功能。比如登录：需要输入账号、密码、点击登录三个操作

## PO模式 V2版本

1. 先定义一个工具类： （创建类和方法时，不需要在方法内传入“self”参数，这样后期调用就不必创建对象了）
2. 再方法封装

**Utils**

``` 
from selenium import webdriver

def get_text():
	return DriverUtils.get_driver().find_element_by_class_name("xxxxxx")


class DriverUtils():
	
	__driver = None  # 私有化，防止在使用工具时，在不调用get_driver情况下，driver为None，出现错误
	
	@staticmethod  # 当方法中 既不需要使用实例对象(如实例对象，实例属性)，也不需要使用类对象 (如类属性、类方法、创建实例等)时，定义静态方法.使用静态方法，可以减少不必要的内存
	def get_driver():
		if driver is not None:
			DriverUtils.__driver = webdriver.Firefox()
			DriverUtils.__driver.maximize_window()
			DriverUtils.__driver.implicitly_wait(30)
			DriverUtils.__driver.get(url)
		return DriverUtils.__driver
	
	@staticmethod
	def quit_driver():
		DriverUtils.__driver.quit()

```



**方法封装**

``` 
from selenium import webdriver
import unittest
import Utils
from Utils import DriverUtils


class TestLogin(unittest.TestCase):
	@classmethod
	def setUpClass(cls):
    	cls.driver = DriverUtils.get_driver()

	@classmethod
	def teaDownClass(cls):
		DriverUtils.quit_driver()
		
	def setUp(self):
		self.driver.get("http://192.168.19.21/index.php")

    def test_login_username_is_error(self):
        self.driver.find_element_by_link_text("登录").click()
        self.driver.find_element_by_id("username").send_keys("13099999999")
        self.driver.find_element_by_id("password").send_keys("123456")
        self.driver.find_element_by_id("verify_code").send_keys("8888")
        self.driver.find_element_by_name("sbtbutton").click()

        msg = utils.get_text()
        print("msg=",msg)
        self.assertIn("账号不存在",msg)

    def test_login_pwd_is_error(self):
        self.driver.find_element_by_link_text("登录").click()
        self.driver.find_element_by_id("username").send_keys("13012345678")
        self.driver.find_element_by_id("password").send_keys("12345678")
        self.driver.find_element_by_id("verify_code").send_keys("8888")
        self.driver.find_element_by_name("sbtbutton").click()
        msg = utils.get_text()
        print("msg=", msg)
        self.assertIn("密码错误", msg)
```



## PO模式 v6版本：

除了封装工具类、分层等外，还把共同操作提取封装到父类中，子类直接调用父类的方法，避免代码冗余。

![PO模式](E:\作业\PO模式.png)



### 工具类

----









### PageObject

-------

每个页面都有单独的pageobject文件。













## 小知识点

``` 
小知识点：(web自动化测试引入的模式及用到的主流测试工具)
	PO模式
        1.PO模式通过对界面元素的封装减少冗余代码
        2.在后期维护中，如果元素定位发生变化，只需要改动页面元素封装的代码，提高测试用例的维护性、可读性
     unittest测试框架：（专门用来进行执行代码测试的框架）
     	1.unittest测试框架方便测试用例的管理
     	2.提供了丰富的断言
     	3.可以生成html测试报告
     selenium Web自动化测试工具，主要做功能测试
```



