.. _var-exp:

变量和表达式
====================

Lua是动态类型语言，变量使用不要预先类型定义。

Lua中有8个基本类型分别为：nil、boolean、number、string、userdata、function、thread和table。

nil / boolean
--------------------
nil为Lua中的特殊类型，可以理解为空，全局变量没有赋初值前默认为nil；

boolean有两个值，false和true；

注：在控制结构的条件中除了false和nil为假，其他值都为真。所以Lua认为0和空串都是真。

number / string
--------------------
可以使用单引号或者双引号表示字符串::

    a = "one string"
    b = string.gsub(a, "one", "another")  -- change string parts 
    print(a)   --> one string 
    print(b)   --> another string

可以使用[[...]]表示字符串。这种形式的字符串可以包含多行。

这种形式的字符串用来包含一段代码非常方便。
::

    page = [[ 
    <HTML> 
    <HEAD> 
    <TITLE>An HTML Page</TITLE> 
    </HEAD> 
    <BODY> 
    Lua 
    [[a text between double brackets]] 
    </BODY> 
    </HTML> 
    ]] 
    io.write(page)

类型转换
^^^^^^^^^^^^^^^^^^^^
运行时，Lua会自动在string和numbers之间自动进行类型转换，当一个字符串使用算术操作符时，string就会被转成数字。
::

    print("10"+ 1)     --> 11 
    print("10 + 1")  --> 10 + 1 
    print("-5.3e - 10" * "2")  --> -1.06e-09 
    print("hello"+ 1)    -- ERROR (cannot convert "hello")
   
当然，类型不同的值，肯定还是不相等；例如::

    a = "10"
    b = 10
    print (a == b)  --> false
    使用转换函数就好：
    print( tonumber(a) == b) --> true
    print( a = tosring(b))  -> true

function
--------------------
函数是第一类值（和其他变量相同），意味着函数可以存储在变量中，可以作为函数的参数，也可以作为函数的返回值。

userdata
--------------------
userdata可以将C数据存放在Lua变量中，userdata在Lua中除了赋值和相等比较外没有预定义的操作。userdata用来描述应用程序或者使用C实现的库创建的新类型。例如：用标准I/O库来描述文件。

thread 线程
--------------------
(waiting...)

table 表
--------------------
表可以理解为数组::

    t = {"a","b","c"}   --> 初始化
    print(t[1])  -->  a   注意：Lua的表的首个位置下标是1 ，而不是0

还可以直接定义并初始化内部结构，类似定义一个结构体，然后初始化::

    a = {x=0, y=0}   -->   a = {}; a.x=0; a.y=0

在构造函数中域分隔符逗号（","）可以用分号（";"）替代，通常我们使用分号用来
分割不同类型的表元素::

    t = {x=10, y=45; "one", "two", "three"} 

连接符和注释
--------------------
..在Lua中是字符串连接符，当在一个数字后面写..时，必须加上空格以防止被解释
错。(如果操作数为数字，Lua将数字转成字符串)
::
    > print("hello " .. "lua")
    hello lua
    > print(10 .. 20)
    1020

表达式
--------------------
1. 关系运算符和算术运算符和C语言/JAVA类似；
#. 二元运算符：+ - * / ^   (加减乘除幂) 
#. 一元运算符：- (负值) 
#. 关系运算符：<  >  <=  >=  ==  ~= （唯一不同的是不等于使用 ~=）
#. 逻辑运算符：and or not 

.. note::

    逻辑运算符认为false和nil是假（false），其他为真，0也是true.
    and和or的运算结果不是true和false，而是和它的两个操作数相关。
    a and b   -- 如果a为false，则返回a，否则返回b 
    a or b   -- 如果a为true，则返回a，否则返回b

例如::

    print(4 and5)  --> 5 
    print(nil and13)   --> nil
    print(4 or5)    --> 4 

实用技巧::
    
    x = x or v 
    等价于
    if not x then
        x = v 
    end

