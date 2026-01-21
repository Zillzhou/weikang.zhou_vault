# 使用 pytest 编写用例

## 使用 pytest 编写用例
## pytest 简介
引用官方文档的介绍: pytest框架使编写小型可读测试变得容易，并且可以扩展以支持应用程序和库的复杂功能测试。pytest是一个成熟的功能齐全的Python测试工具，可以帮助您编写更好的程序。

💡 Tips：pytest需要 Python 3.7+或PyPy3。
## 本测试文档使用的插件版本
## pytest 7.3.1
## pytest-html 3.2.0

## 快速使用
import pytest  # pytest是第三方库，需要导入
## import sys
## import os
### p = os.path.abspath(__file__)
### p = os.path.dirname(p)
### p = os.path.dirname(p)
### p = os.path.dirname(p)
sys.path.append(p)      # 防止路径变化时导入APILib失败
from APILib.orderLib import *   # APILib是一个core的内部测试库

## core :OrderLib=None
## def setup_module():
### """执行这个脚本的用例前需要的准备内容"""
## global core
    core = OrderLib(getServerAddr())
### p = os.path.abspath(__file__)
### p = os.path.dirname(p)
    p = os.path.join(p， "scene.zip")
### print("setup_module ...")
### core.uploadScene(p) # 加载场景
## time.sleep(3)

### def teardown_module():
### """执行这个脚本的用例后需要执行内容"""
## global core
### core.recoveryParam()

## def test_demo():
## """"""
## global core
## if core is None:
        core=OrderLib(getServerAddr())
## res=0
## """do some test"""
## if res>0:
## assert False

### if __name__ == "__main__":
    pytest.main(["-v"， "--html=report.html"，"-k test_demo"， "--self-contained-html"])
### 执行 main 函数或命令行输入 pytest 运行用例
## 运行结果:
1700789109267-de053c65-e6e8-4db3-8362-90349c026fee.png

### 使用 pytest 编写用例，必须遵守以下规则:
测试文件名必须以 test_ 开头或者 _test 结尾(如: test_ab.py )
测试方法必须以 test_ 开头。
测试类命名以 Test 开头。

## 用例目录结构
1701135507702-1927f5fb-3c19-40e8-9b15-680afbddc37d.jpeg

### orderLib.py 调度相关的测试库
## rbklib.py 单车相关的测试库
### config.json 测试环境相关配置
### conftest.py pytest自定义功能
## report 临时报告存放处
### pytest.init pytest配置

## 用例编写
## setup teardown结构
setup 用于处理测试前期准备， teardown 用于清除被污染的测试环境
setup/teardown 有四个级别，分别为 module ， class ， method and function
module 级别将会在执行模块前(后)被调用一次，使用方法名setup_module/teardown_module定义
class 级别将会在所有类方法之前(后)调用一次，所有类方法之后调用一次，使用时将setup_class/teardown_class作为类方法定义
method 级别将会在每个类方法前(后)调用，使用时定义为类的普通方法
### function 级别会对每一个模块中的函数之前(后)调用
### 💡 Tips：以下情况 teardown 将不会被调用
## 相关的 setup 执行失败
## 用例被 skip

强烈建议每个模块要有 setup_module ， teardown_module
用例编写完需要检查 setup_module 或 teardown_module :
## 是否有上传场景和模型文件
## 是否有恢复默认参数
## 是否有清除未执行的运单
## 是否有启用禁用的点位或线路
## 是否有触发火警未关闭
## 是否有清除报错
## 断言
pytest 使用 Python assert 语句进行断言
## 语法格式如下所示：
## assert 表达式
assert用于判断某个表达式的值，结果为 True，程序运行， 否则，程序停止运行，抛出 AssertionError 错误 。
### 每个用例至少有一个断言， 若果没有，需明确注释
断言示例: https://docs.pytest.org/en/7.4.x/example/reportingdemo.html#tbreportdemo
## fixture
## yield fixtures
使用 yeild fixture 实现 setup ， teardown 功能，下述案例中，如果不了解 yeild ，可以理解为在每个 function 之前运行 yeild 之前的代码，每个 function 之后运行yeild之后的代码.
@pytest.fixture(scope="function"， autouse=True)
### def reset_bin_state():
    set_bin_status("WS-1-5"， False)
    set_bin_status("WS-1-4"， False)
### core.dispatchable("sim_02")
## yield
### core.terminateIdList([])
当有多个 fixture 时， pytest 将计算 fixture 的线性顺序，直到每个 fixture 返回，详细示例见
https://docs.pytest.org/en/7.4.x/how-to/fixtures.html#yield-fixtures-recommended
## 注释
## 开头注释
应写在""" """，这将存储在函数的__doc__属性中， 开头注释尽量多写点儿
## def test_success():
## """创建订单成功
## 此需求:coding链接
## 预期结果:状态码200，msg为ok
## """
## 断言注释
断言注释可以在测试失败时快速判断失败原因.
assert a % 2 == 0， "value was odd， should be even"
## pytest相关
pytest.skip("reason")                     # 在函数中使用
### @pytest.mark.skip("reason")
@pytest.mark.xfail("reason")  # 预计失败
## 标记
## @pytest.mark
## @pytest.mark.m0
## def test_sever():
## """标记函数"""

## @pytest.mark.m0
## class TestClass:
## """标记类"""

## import pytest
### pytestmark = pytest.mark.m0
### """使用 pytestmark 标记模块"""
## 注册标记
## 标记可以将用例分类，有助于选择运行用例
## 在pytest.init中注册标记
## [pytest]
## markers=
    m0: Mark the important cases of rdscore as m0 and execute them when a new version is released
    mapf: Mark the important cases of MAPF as mapf and execute them when a new version is released
    slow: Mark cases that run slowly for more than 3 minutes as slow
    imperfect: Mark cases that always fail in overall testing

## 使用未注册的标记将被警告
使用 @pytest.mark 装饰用例时，如果标记未注册，为避免异常行为，pytest的做法是运行用例并输出警告
如果想查看哪些用例使用了未知标记，可以使用命令参数 --strict-markers ，此时未注册的标记将触发异常
可以在命令行输入 pytest --markers 查询已定义的标记
## 命令行使用 pytest
## 指定运行用例
## 运行模块下的所有用例
### pytest test_addBlock.py
## 运行目录下的所有用例(递归模式)
### pytest test-rdscore/
## 模块下的某一用列
pytest test__addBlock.py::test_1
pytest test__addBlock.py::test_2[1，2]        # test_2需要传入两个参数
## 匹配特殊的测试用例
## -k
运行包含字符串表达式匹配的用例(不区分大小写)， 表达式可以使用 and ， or ， not ， ()
pytest -k 'test_s and not test_success'
## -m
运行含表达式标记的用例，表达式可以使用 and ， or ， not ， ()
### pytest -m 'm0 and not slow'

## 输出格式
-v verbosity 显示测试进度， 断言失败时的详细内容， 夹具详细内容等.
## -vv 更详细的测试内容
-q quiet 降低详细程度，一般只显示测试成功或失败结果
-s 等价于 --capture=no ，此策略将不会捕获日志
### -x exitfirst 在第一个错误或者失败时退出
--sw 在测试失败时退出，且下次会从失败的用例开始测试，这对排查用例本身问题很有帮助
### --coellct-only 收集用例，不执行
--maxfail 自定义多少次失败后停止，如 --maxfail=2 ， 需要插件 pytest-rerunfailures
--html 输出测试报告，需要插件 pytest-html
--self-contained-html 测试报告本身包含html样式，需要插件 pytest-html
## 常见问题
## 命令行运行的常见策略
测试用例存在失败情况，此时期望允许在修复脚本后，测试从上次失败的测试继续
### 需要在每次调用 pytest 时包含参数 --sw
pytest --sw -v -m m0 --html=report.html
💡 pytest只会缓存最近一次测试进度和日志信息，若要清除缓存，使用 --cache-clear 

### 运行 m0 但不运行 imperfect 标记的测试用例
pytest -v -m "m0 and not imperfect" --html=report.html --self-contained-html
## pytest.init
## addopts
### addopts参数可以更改默认命令行选项
addopts = -v --reruns=1 --count=2 --html=reports.html --self-contained-html -n=auto
## markers
## 注册标记
## [pytest]
## markers =
## m0: description
## norecursedirs
## 类似gitignore，可以忽略目录
norecursedirs = .* build dist CVS _darcs {arch} *.egg 
## 测试时指定运行版本
### 不同版本的功能可能有细微差异，某些用例只能在特定版本运行
## 在夹具中获取版本
@pytest.fixture(scope="function"，autouse=True)
## def get_core(core):
## global core_v
## if core == "":
        raise EnvironmentError("core version error")
## else:
## core_v=core
        print("\tcore:version:"，core_v)
## 在用例中判断版本
## 命令行运行
### pytest -v --core=0.1.9.231121
## 如何一个用例多次输入不同参数
@pytest.mark.parametrize 装饰器可以将多个参数依次传递给测试函数
下述案例会将每一个test_input与expected作比较，一共有三组  (test_input，expected)，函数test_eval也会运行三次
@pytest.mark.parametrize("test_input，expected"， [("3+5"， 8)， ("2+4"， 6)， ("6*9"， 42)])
def test_eval(test_input，expected):
## """测试..."""
### assert test_input== expected

## 参数的笛卡尔积
@pytest.mark.parametrize("x"， [0， 1])
@pytest.mark.parametrize("y"， [2， 3])
## def test_foo(x， y):
          """测试参数(x，y):(0，2)，(1，2)，(0，3)，(1，3)""
## pass

## 可以为单独的某一条测试添加标记
@pytest.mark.parametrize("test_input，expected"，[("3+5"， 8)， ("2+4"， 6)， pytest.param("6*9"， 42， marks=pytest.mark.xfail)]，)
def test_eval(test_input， expected):
## 如何设置超时
用例可能采用 while true 的设计，这可能会导致超时，下面是
## 使用插件 pytest-timeout
## 装饰器
### @pytest.mark.timeout(15)
## def test_foo():
## """超时15s将失败"""
## 命令行
## pytest --timeout=2
缺点: pytest-timeout 采用额外线程处理超时，当出现超时将会终止pytest运行而不是跳过超时用例.
## 自定义超时
### 用例中使用OrderLib.timeOut()
## def test_foo()
### core=OrderLib(getSeverAddr())
### core.time_tem=time.time()
    ...
## core.timeOut()

### 用例超时将会抛出异常，而pytest可以跳过异常用例
## 尽量少些 while true 循环
## while else处理循环正常退出
某些时候，我们希望循环是因 break 退出而不是正常退出，此时可以运用 python 的 while...else 结构
## def test_foo():
## """测试.."""
## while expression1:
## time.sleep(1)
## if expression2:
## break
## else:
## assert False
## 上述代码当循环正常退出时断言失败
## 类似的还有 for...else 循环
## 如何清理测试环境
## 使用 teardown
### def teardown_module():
## """在所有用例完成后清理"""
## global ORDER
### p = os.path.abspath(__file__)
### p = os.path.dirname(p)
    mp = os.path.join(p， "default.model")
    ORDER.rbk.robot_config_model_req(mp)  #上传正确的模型文件
## time.sleep(5)
    ORDER.recoveryParam()                                             # 恢复参数
    ORDER.enablePath(name="all")                                # 启用路径
    ORDER.enablePoint(name="all")                                # 启用点位
## 使用 yeild fixture
@pytest.fixture(scope="function"， autouse=True)
### def reset_bin_state():
## """在每个函数执行完后清理"""
    set_bin_status("WS-1-5"， False)
    set_bin_status("WS-1-4"， False)
### core.dispatchable("sim_02")
## yield
### core.terminateIdList([])
## 冒烟测试时运行慢
增加参数 --durations=10 可以查看运行时长最长的10个用例，或在html报告中查找运行慢的用例
## 检查是否有全局对象提前初始化
## 检查循环否没有提前break
## 增加仿真速度
## 优化断言方式
## 测试报告中文乱码
### 修改pytest-html->result.py
1701155718491-d5e67764-a3f7-47cb-ba4d-7bcb9c7d0e14.png
