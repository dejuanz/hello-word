[TOC]

# TestSuite和defaultTestLoader的区别

- TestSuite可以添加TestCase中所有以“test”开头的方法和添加指定的以“test”开头的方法。
- defaultTestLoader搜索指定文件目录下指定开头的.py文件，并添加TestCase中所有以“test”开头的方法，不能指定添加方法。
- defaultTestLoader属于TestSuite另一种实现方式。
- defaultTestLoader中的discover类，脚本执行顺序不可控制（根据类、方法名先后顺序），TestSuite中的addTest可以控制顺序

## TestSuite（测试套件）

多条测试用例集合在一起，就是一个TestSuite

### 步骤

``` 
1.实例化
	suite = unittest.TestSuite()
2.添加用例
	suite.addTest(ClassName("MethodName"))  # 类名（方法名）
3.添加扩展
	suite.addTest(unittest.makeSuite(ClassName))  # 搜索指定ClassName内test开头的方法，并且添加到测试套件中
4.实例化运行器
	runner = unittest.TextTestRunner()
5.执行
	runner.run(suite)
	
	
***特别提示***
	1.一条测试用例（.py）内，多个方法也可以使用测试套件
	2.TestSuite需要配合TextTestRunner才能被执行
```



## defaultTestLoader

使用unittest.defaultTestLoader()类，通过该类下面的discover方法自动搜索指定目录下指定开头的.py文件，并将查找到的测试用例组装到测试套件

### 步骤

``` 
1.测试脚本路径
	test_dir = './'
2.创建测试套件
	discover = unittest.default.TestLoader.discover(test_dir,pattern='test_*.py')
	# pattern 为查找的.py文件的格式：以什么开头的.py文件
3.创建运行器
	runner = unittest.TextTestRunner()
	runner.run(discover)
```



# 测试报告生成步骤

``` 
1.创建一个Tool文件夹 ，并且将HTMLTestRunner.py文件复制到该目录下
2.导入HTMLRunner、unitTest包
3.discover加载要执行的用例
	discover = unittest.defaultTestLoader.discover(test_dir, pattern = 'test*.py')
4.设置 报告生成路径和文件名：
	file_name = file_dir + nowtime + "report.html"
5.打开报告:
	with open(file_name. "wb") as f:
6.实例化HTMLTestRunner对象：
	runner = HTMLTestRunner(stream = f,[title],[description])
7.执行：
	ruuner.run(descover)
	
***特别小知识点***
	1.stream:文件流，打开写入报告的名称及写入编码格式
	2.[],为可选；title为报告标题，比如xxx自动化测试报告
	3.description:为说明，比如操作系统、浏览器版本
```

















