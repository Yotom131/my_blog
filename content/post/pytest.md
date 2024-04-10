+++

title = 'pytest'
date = 2024-04-10T12:50:10+08:00
author = "Yotom"
description = "一个基于python的测试框架"
tags = [
    "测试方法",
    "自动化测试",
    "测试基础"

]
categories = [
    "学习记录",
    "测试开发",
]

+++

# pytest

## 测试用例命名规范

<div style="text-align: center">
    <img src="/img/pytest_1.png" alt"p1" style="max-width: 100%; height: auto;">
</div>

### 示例

```python
class TestXXXX:
    def setup(self):
        # 资源准备
        pass

    def teardown(self):
        # 资源销毁
        pass

    def test_XXX(self):
        # 测试步骤1
        # 测试步骤2
        # 断言  实际结果  对比  预期结果
        assert ActualResult == ExpectedResult
```

---

## 测试类结构

<div style="text-align: center">
    <img src="/img/pytest_2.png" alt"t2" style="max-width: 100%; height: auto;">
</div>

---

## 参数化测试函数

### 单参数

```python
import pytest
search_list = ['appium', 'selenium', 'pytest']

@pytest.mark.parametrize('name', search_list)
def test_search(name):
    assert name in search_list
```

**本文无需要完全掌握修饰器原理**

**但如果对于修饰器理解不透彻，想要了解其原理，可以参考[B站视频](https://www.bilibili.com/video/BV1Gu411Q7JV)**

### 多参数

```python
import pytest
@pytest.mark.parametrize("test_input,expected",[
    ("3+5",8),("2+5",7),("7+5",12)
], ids=['num1','num2','num3'])# 这里通过ids对测试用例进行了重命名
def test_mark_more(test_input,expected):
    assert eval(test_input) == expected
    # 这里的eval()可以将括号内的字符串转化为表达式
    # 比如eval("10+20")等价于30
```

需要注意修饰器里的参数名要与函数里的参数名一一对应

如果传递多个参数，要将参数放在列表中，利用列表中嵌套列表/元组

### 笛卡尔积

```python
a = [1, 2, 3]
b = [a, b, c]
```

请问上述列表中元素两两组合，有多少种元素呢？

```python
(1,a),(1,b),(1,c)
(2,a),(2,b),(2,c)
(3,a),(3,b),(3,c)
```

根据两两组合，让测试更加全面，可以按照全组合来全面排查问题。

```python
@pytest.mark.parametrize("word",["appium","selenium","pytest"])
@pytest.mark.parametrize("code",["utf-8","gbk","gb2312"])
def test_dkej(word, code):
    print(f"word:{word}, code:{code}")
```

---

## 标记测试用例

+ 场景：只执行符合要求的某一部分用例，可以把一个web项目划分为多个模块，然后指定模块名称执行。

+ 解决：在测试用例方法上加上@pytest.mark.标签名（标签名使用英文）

+ 执行：-m执行自定义标签的相关用例

  ```python
  pytest -s test_mark_zi_09.py -m=webtest
  pytest -s test_mark_zi_09.py -m apptest# 此条等价于上条
  pytest -s test_mark_zi_09.py -m "not ios"# 此条排除了IOS用例
  ```

### 示例

```python
def double(a):
    return a*2
# 测试类型：整型
@pytest.mark.int
def test_double_int():
	print("test double int")
    assert 2 == double(1)
    
# 测试类型：负数
@pytest.mark.minus
def test_double_minus():
	print("test double minus")
    assert -2 == double(-1)
    
# 测试类型：浮点数
@pytest.mark.float
def test_double_float():
	print("test double float")
    assert 0.2 == double(0.1)
    
@pytest.mark.float
def test_double2_float():
	print("test double float")
    assert -0.2 == double(-0.1)
    
@pytest.mark.zero
def test_double_0():
	print("test double float")
    assert 0 == double(0)
    
@pytest.mark.str
def test_double_str1():
	print("test double float")
    assert "aa" == double("a")
    
@pytest.mark.str
def test_double_str2():
	print("test double float")
    assert "a$a$" == double("a$")
```

---

## 跳过（Skip）和预期失败（xFail）

pytest内置了一些标签，可以处理一些特殊的测试用例、不能成功的测试用例。

+ skip：始终跳过该测试用例
+ skipif：遇到特定情况跳过该测试用例
+ xfail：遇到特定情况，产生一个“期望失败输出”

### 示例

```python
# skip
@pytest.mark.skip(reason="代码未实现")
def test_aaa():
    assert False
    
# 代码中添加，跳过代码块pytest.skip(reason="")
def check_login():
    return True
    
def test_function():
    print("start")
    # 如果未登录，则跳过后续步骤
    if check_login():
        pytest.skip("暂未支持")
    print("end")
    
# skipif
@pytest.mark.skipif(sys.platform == 'win', reason="does not run on windows")
def test_case1():
    assert True
    
@pytest.mark.skipif(sys.version_info < (3, 6), reason="requires python3.6 or higher")
def test_case2():
    asser True
    
# xfail
@pytest.mark.xfail
def test_aaa():
    pritn("test_xfail1 执行")
    assert 2 == 2
    
xfail = pytest.mark.xfail
@xfail(reason="bug 110")
def test_hello():
    assert 0

def test_xfail():
print("*****start*****")
pytest.xfail(reason='该功能未完成')
print("测试过程")
assert 1 == 1
```

---

## 运行用例

待完成（4.10 17：39）