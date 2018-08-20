[TOC]

# Web自动化测试

**定义：**让程序代替人为自动验证Web项目功能的过程。

**适合做自动化测试的Web项目：**

- 需求变动不频繁
- 项目周期长
- 项目需要回归测试
  - 项目在发新版本之后对项目之前的功能进行验证。

# 自动化测试分类

1.按可见度分：

- 黑盒测试（功能测试）
- 灰盒测试（接口测试 ）
- 白盒测试（单元测试）

**注意：**Web自动化测试属于黑盒测试（功能测试）

2.按测试种类：

- Web(UI)自动化测试
- 接口自动化测试
- 移动自动化测试
- 单元测试自动化测试

## 自动化测试优点

- 较少时间内运行更多的测试用例
- 自动化脚本可以重复运行
- 减少人为错误
- 测试数据存储

## 自动化测试缺点

- 不能取代手工测试
- 手工测试比自动化测试发现的缺陷更多
- 测试人员技能要求

# Selenium家族史

**selenium 1.0:**

- selenium IDE
  - Firefox浏览器插件，可以录制用户的基本操作，生成测试用例
  - 测试用例可以在Firefox浏览器里回放
  - 测试用例可以转化成其他语言的自动化测试脚本
- selenium Grid
  - 允许selenium RC针对规模庞大的测试用例集或者需要在不同环境中运行的测试用例集进行扩展
- selenium RC
  - RC是remote control的缩写，它的功能就是用来模拟一个浏览器
  - 支持多平台、多浏览器、多种开发语言 编写测试用例
  - 不支持本机键盘和鼠标事件
  - 不支持同源策略XSS/HTTP(S)
  - 不支持弹出框、对话框（自签名证书和文件的上传、下载 ）

**selenium 2.0**（与1.0有本质上的区别）

- selenium 2.0 = selenium 1.0 + **WebDriver**（通常说WebDriver即selenium 2.0）
- 基于调用WebDriver API来模拟用户操作
- WebDriver 的速度更快，因为他可以直接交互使用
- 支持多种编程语言

**selenium 3.0**(实际是2.0的升级版)

- 去掉了对selenium RC的支持
- 全面支持 Java8
- 支持macOS(sierra or later),支持官方的SafariDriver
- 通过微软的WebDriver Server,支持Edge浏览器
- 支持IE9.0以上的版本
- 通过火狐官方的GeckoDriver来支持Firefox

# Selenium IDE

selenium IDE是一个Firefox的插件，用于记录和播放用户与浏览器的交互（录制web操作脚本）

## selenium IDE常用命令 

- open

- pause

- goBack

- refresh

- click(locator)

- type(locator,value)

- close

  ![selenium IDE](E:\作业\selenium IDE.png)



# WebDriver

- WebDriver是一种用于web应用程序的自动化测试工具；
- 它提供了一套有好的API（应用编程接口说明 ）；
- WebDriver完全就是一套类库，不依赖于任何测试框架，除了必要的浏览器驱动。

## selenium的安装与卸载

1.线上安装

​	打开cmd---->pip install selenium==2.48.0

​	卸载：pip uninstall selenium

​	查看：pip show selenium

2.离线安装

​	进入到selenium安装包路径，打开cmd----->python setup.py install

## 浏览器操作方法

### WebDriver操作浏览器常用方法

1. maximize_window()------------------------------->模拟浏览器最大化按钮
2. set_window_size()---------------------------------->设置浏览器大小（像素）
3. set_window_position()---------------------------->设置浏览器位置
4. back()---------------------------------------------------->模拟浏览器后退按钮
5. forward()------------------------------------------------>模拟浏览器前进按钮
6. refresh()------------------------------------------------->模拟浏览器F5刷新
7. close()---------------------------------------------------->模拟浏览器关闭按钮（关闭单个窗口）
8. quit()------------------------------------------------------>模拟浏览器关闭按钮（关闭全部窗口）



## webdriver--API

## 元素定位

1. id
2. name
3. class_name
4. tag_name
5. link_name
6. partial_link_name
7. XPath
8. CSS

``` 
# 导包
from selenium import webdriver
from time import sleep

# 实例化火狐浏览器
driver = webdriver.Firefox()
# 准备url
url = 'file:///E:/%E7%AC%AC%E4%BA%94%E9%98%B6%E6%AE%B5_web/day1/%E9%A2%84%E4%B9%A0%E8%B5%84%E6%96%99/_book/02img/%E6%B3%A8%E5%86%8CA.html'
# 打开url
driver.get(url)

#调用定位方法
# id方法
driver.find_element_by_id('userA').send_keys('admin')
sleep(1)
# name方法
driver.find_element_by_name('userA').send_keys('admin')
sleep(1)
# class_name方法
driver.find_element_by_class_name('telA').send_keys('18611111111')
sleep(1)
# tag_name方法
driver.find_elements_by_tag_name('input')[4].send_keys('admin')
sleep(1)
driver.find_elements_by_tag_name('input')[5].send_keys('123456')
sleep(1)

# xpath方法
driver.find_element_by_xpath('/html/body/form/div/fieldset/p[6]/input').send_keys('345')  # 绝对路径"/"开头
# xpath方法
driver.find_element_by_xpath('//input').send_keys('345')  # 绝对路径"/"开头
sleep(1)
driver.find_element_by_xpath('//*[@id="emailA"]')
sleep(1)
# link_name方法
driver.find_element_by_link_text('访问 新浪 网站').click()
sleep(1)

# partial_link_name方法
driver.find_element_by_partial_link_text('访问').click()
sleep(1)


sleep(3)
driver.quit()
```



## XPath定位方法

### 1.路径

- 绝对路径：以"/"开头
- 相对路径：以"//"开头

### 2.属性值

- **例：**//*[@id='kw']
- 

### 3.层级与属性结合

- **例：**//*[@id='p1']/input
- **注意：**在要定位的元素没有属性值，上级有属性值时使用。

### 4.属性与逻辑关系结合

- **例：**//*[ @class='userA' and @name='userA']
- **注意：**在属性值不唯一时使用。

## CSS元素定位方法

### 1. id选择器

- #userA

### 2. class选择器

- .userA

### 3. 元素选择器

- input

### 4. 属性选择器

- **例：**[id='userA']

### 5. 层级选择器

- **例：**p>input
- **例：**p>[type='userA']

## 元素操作方法

### 元素常用操作方法

1.clear()----------------------------------------->清除文本

2.send_keys()--------------------------------->模拟输入

3.click()------------------------------------------>单击元素





## WebDriver其他常用方法

1. size------------------------------------------------------>元素大小
2. text------------------------------------------------------>元素的文本内容
3. title------------------------------------------------------>当前页面的title
4. current_url------------------------------------------->当前页面的URL
5. get_attribute('xxx')-------------------------------->获取元素'xxx'属性的属性值
6. is_display--------------------------------------------->判断元素是否可见
7. is_enable--------------------------------------------->判断元素是否可用



# 鼠标事件

WebDriver中操作鼠标的方法封装在ActionChains类中，路径可以不记，可以利用Ctrl + Alt + Space 键导入

1. context_click()               右击---->模拟鼠标右键点击效果
2. double_click()                双击---->模拟鼠标双击效果
3. drag_and_drop()           拖动---->模拟鼠标拖动效果
4. move_to_element()      悬停---->模拟鼠标悬停效果
5. perfrom()                       动作执行

## 鼠标事件执行步骤

``` 
# 导包
from selenium import webdriver
from time import sleep
from selenium.webdriver.common.action_chains import ActionChains

# 实例化火狐浏览器对象
driver = webdriver.Firefox()
url = "https://www.baidu.com"
driver.get(url)

# 实例化鼠标动作
action = ActionChains(driver)

# 封装鼠标动作
# ActionChains()对象.操作鼠标方法（element定位）
action.context_click(driver.find_element_by_name("tj_trnews"))

# 执行动作
action.perform()

sleep(5)
driver.close()
```



# 键盘操作

WebDriver中键盘操作封装在Keys类中

``` 
# 导包
from selenium import webdriver
from time import sleep
from selenium.webdriver.common.keys import Keys

# 实例化火狐浏览器对象
driver = webdriver.Firefox()
url = "https://www.baidu.com"
driver.get(url)

# 在输入框中输入“淘宝”，并利用键盘输入空格
driver.find_element_by_id("kw").send_keys("淘宝")
driver.find_element_by_id("kw").send_keys(Keys.SPACE)
# 全选、复制输入框中全部内容，并粘贴到输入框中
driver.find_element_by_id("kw").send_keys(Keys.CONTROL,'a')
driver.find_element_by_id("kw").send_keys(Keys.CONTROL,'c')
driver.find_element_by_id("kw").send_keys(Keys.CONTROL,'v')

sleep(5)
driver.quit()
```



# 元素等待

**元素等待：**WebDriver定位页面元素时如果没有找到，会在指定时间内一直等待过程。

什么时候需要用到元素等待：

- 网络速度限制
- 电脑配置限制
- 服务器处理请求原因

## 显示等待

**显示等待：**使WebDriver等待指定元素条件成立时继续执行，否则达到最大时长抛出超时异常(TimeoutExceotion)

显示等待的相关方法封装在WebDriver中的**WebDriverWait**中，相关判断方法封装在exception_conditions类中

``` 
# 导包
from selenium import webdriver
from time import sleep
from selenium.webdriver.support.wait import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC  # 判断element定位是否准确方法类
from selenium.webdriver.common.by import By
from selenium.common.exceptions import TimeoutException  # 抛出异常

# 实例化火狐浏览器
driver = webdriver.Firefox()
url = "https://www.baidu.com"
driver.get(url)

## 显示等待   located 处于、位于
#WebDriverWait(driver,5).until(EC.presence_of_element_located((By.ID,"sua"))).click()
#sleep(3)
#driver.quit()

# 捕获异常
try:
	WebDriverWait(driver,5).until(EC.presence_of_element_located((By.ID,'sua')))
except TimeoutException as result:
	print(result)
finally:
	sleep(3)  
	driver.quit()  #写在finally下面可以确保执行

```



## 隐式等待

**隐式等待：**等待元素加载指定时长，超出抛出` NoSuchElementException `异常，实际工作中，一般都使用隐式等待

- 显示等待为单个元素有效，隐式等待为**全局元素**
- 显示等待封装在WebDriverWait类中，而隐式等待则直接通过浏览器实例化对象调用，不需要导包。

``` 
# 导包
from selenium import webdriver
from time import sleep
from selenium.common.exceptions import NoSuchElementException


# 实例化火狐浏览器
driver = webdriver.Firefox()
url = "https://www.baidu.com"
driver.get(url)

# 设置隐式等待
driver.implicitly_wait(5)

try:
    driver.find_element_by_id("kwa").send_keys("淘宝")
except NoSuchElementException as result:
    print(result)
finally:
    sleep(3)
    driver.quit()
```



# 下拉选择框、警告框、滚动条操作

## 下拉选择框

``` 
from selenium import webdriver
from time import sleep
from selenium.webdriver.support.select import Select

#实例化火狐浏览器
driver = webdriver.Firefox()
url = "https://www.baidu.com"
driver.get(url)

driver.find_element_by_link_text("设置").click()
driver.find_element_by_link_text("搜索设置").click()
# 实例化Select对象
select = Select(driver.find_element_by_id("nr"))
select.select_by_value("50")

sleep(5)
driver.quit()
```

## 警告框

警告框：元素定位定位不到；样式比较单一

``` 
from selenium import webdriver
from time import sleep

# 实例化火狐浏览器
driver = webdriver.Firefox()
url = "file:///E:/%E7%AC%AC%E4%BA%94%E9%98%B6%E6%AE%B5_web/day3/01-%E8%B5%84%E6%96%99/%E9%A2%84%E4%B9%A0%E8%B5%84%E6%96%99/_book/02img/%E6%B3%A8%E5%86%8CB.html"
driver.get(url)

# 定位警告框按钮
driver.find_element_by_id('alertB').click()

# 获取警告框
m_alert = driver.switch_to.alert

m_alert.accept()

sleep(5)
driver.quit()
```

## 滚动条

``` 
from selenium import webdriver
from time import sleep

# 实例化火狐浏览器
driver = webdriver.Firefox()
url = "https://www.taobao.com/"
driver.get(url)

# 设置JavaScript脚本控制滚动条
js = "window.scrollTo(0,10000)"
# WebDriver调用js脚本方法
driver.execute_script(js)

sleep(5)
driver.quit()
```

## frame表单切换

``` 
from selenium import webdriver
from time import sleep

# 实例化火狐浏览器
driver = webdriver.Firefox()
url = "file:///E:/%E7%AC%AC%E4%BA%94%E9%98%B6%E6%AE%B5_web/day3/01-%E8%B5%84%E6%96%99/%E9%A2%84%E4%B9%A0%E8%B5%84%E6%96%99/_book/02img/%E6%B3%A8%E5%86%8C%E5%AE%9E%E4%BE%8B.html"
driver.get(url)

driver.find_element_by_id("user").send_keys("admin")
driver.switch_to.frame("idframe1")  # driver.switch_to.frame("frame表单的id或者name")
driver.find_element_by_id("userA").send_keys("admin")
driver.switch_to.default_content()
driver.switch_to.frame("myframe2")
driver.find_element_by_id("userB").send_keys("admin")

sleep(5)
driver.quit()
```

## 获取窗口句柄

``` 
from selenium import webdriver
from time import sleep

# 实例化火狐浏览器
driver = webdriver.Firefox()
url = "file:///E:/%E7%AC%AC%E4%BA%94%E9%98%B6%E6%AE%B5_web/day3/01-%E8%B5%84%E6%96%99/%E9%A2%84%E4%B9%A0%E8%B5%84%E6%96%99/_book/02img/%E6%B3%A8%E5%86%8CA.html"
driver.get(url)


print(driver.current_window_handle)
driver.find_element_by_id("ZCA").click()
print(driver.window_handles)

handles = driver.window_handles
driver.switch_to.window(handles[1])
driver.find_element_by_id("userA").send_keys("admin")

sleep(5)
driver.quit()
```

## 截图

get_screenshot_as_file("存放图片的路径+图片名+图片格式")  

 #相对路径、如果存放在某文件夹下，需要手动建文件夹

## 验证码

1. 测试环境：去掉验证码、
2. 生产环境和测试环境：设置**万能验证码**
3. 验证码识别框架
4. 利用cookie(绕过登录逻辑，实现对验证码处理：需要几个cookie值需要找开发人员确认)

# UnitTest框架 

**UnitTest框架**是 专门用来进行执行代码的框架。

**为什么用UnitTest框架：**

 - 能够组织多个用例去执行
 - 提供丰富的断言方法：
   - 让程序代替人为判断测试程序执行结果是否符合预期结果的过程
 - 提供丰富的日志与测试结果

## TestCase（测试用例）

- 一个TestCase就是一条测试用例

``` 
import unittest  # 用到UnitTest,就要先导包

class TestCase_web(unittest.TestCase):
    def test_one(self):
        print("测试1")
    def test_two(self):
        print("测试2")
```

## TestSuite(测试套件)

- 多条测试用例集合在一起就是一个TestSuite

``` 
import unittest
from day5_testcase import TestCase_web

suite = unittest.TestSuite()  # 实例化

suite.addTest(TestCase_web("test_one"))  # 添加用例
suite.addTest(TestCase_web("test_two"))  # 添加扩展

ruuner = unittest.TextTestRunner()
ruuner.run(suite)
```



### addTest







### addTest(unitest.makeSuite(类名))









### defaultTestLoader

使用unittest.defaultTestLoader()类，通过该类下面的discover()方法，自动搜索指定目录下指定开头的.py文件，并将查找到的测试用例组装到测试套件。

#### 步骤

```
test_dir = '../Cases/'  # 要执行的测试用例文件在上一级Cases文件夹中
discover = unittest.defaultTestLoader.discover(test_dir,pattern='iweb_*.py')  # pattern是指测试用				例文件名，'iweb_*.py'指以"iweb_"开头，以".py"结尾的文件。
runner=unittest.TextTextRunner()
runner.run(discover)
```

#### defaultTestLoader与TestSuite区别

1. TestSuite可以添加TestCase中所有test开头的方法和添加指定的test开头方法;
2. defaultTestLoader搜索指定目录下指定开头.py文件，并添加TestCase内所有test开头的方法，不能指定添加方法; 

**提示：**defaultTestLoader属于TestSuite的另一种实现方式

## TestTestRunner(测试执行)

- 用来执行测试套件

``` 
import unittest
from day5_testcase import TestCase_web

suite = unittest.TestSuite()

suite.addTest(TestCase_web("test_one"))
suite.addTest(TestCase_web("test_two"))

ruuner = unittest.TextTestRunner()  # 实例化
ruuner.run(suite)  # 执行用例
```



# UnitTest断言

**断言**是指让程序代替人为判断测试程序执行结果是否符合预期结果的过程

**UnitTest断言分类：**

- 基本布尔型断言
  - assertEqual(arg1,arg2,msg=None): 验证arg1=arg2
  - assertIn(arg1,arg2,msg=None): 验证arg1是arg2的 子串，不是则fail
- 比较断言
- 复杂断言



# 生成HTML测试报告

## 生成步骤

- 复制HTNLTestRunner.py文件到项目文件夹

- 导入HTMLTestRunner、UnitTest包

- discover加载要执行的用例

  discover=unittest.defaultTestLoader.discover(test_dir,pattern='test*.py')

- 设置报告生成的路径和文件名：

  - report_dir = './Reports/'
  - now_time = time.strftime("%Y-%m-%d %H\_%M\_%S")
  - report_name = report_dir + now_time + 'report.html'  # 后缀名要明确

- 打开报告写入结果

  - with open (report_name,"wb") as f:

    runner = HTMLTestRunner(stream=f, verbosity=1, title='Triangle测试报告', description='测试平台：			Windows测试浏览器:Firefox 版本:v35.0.1')
    runner.run(discover)

    











