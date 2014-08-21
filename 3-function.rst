.. _3-function:

函数
====================

函数定义
--------------------

函数以function来声明，可以返回多个结果::

    function max_min_num(a)     
        maxvar = a[1]
        minvar = a[1]
        for i,val in ipairs(a) do   -- ipairs为迭代器函数，每次取出一个数组元素
            if val > maxvar then
                maxvar = val
            end
            if val < minvar then
                minvar = val
            end
        end
        return maxvar, minvar
    end

    maxv, minv = max_min_num({23,2,15,49})  -- 返回多个结果,使用多个变量接收

    print("max:" .. maxv .. "/min:" .. minv)

    --> max:49/min:2
   
前面介绍过，函数也是一种变量，既指向函数的变量（C语言里面的函数指针？没错，一样的）::
        
    function x(a) return a*2 end   -->等价于下面这句
    y = function (a) return a*2 end  --> 理解函数即是变量
    print(x(8))
    print(y(8))
    --> 16
    --> 16

可变参数
--------------------
Lua函数可以接受可变数目的参数，和C语言类似在函数参数列表中使用三点（...）表示函数有可变的参数。Lua将函数的参数放在一个叫arg的表中，除了参数以外，arg表中还有一个域n表示参数的个数::

    s = ''
    m = ''
    function my_print(...)   -- 可变参数列表
        for i,v in ipairs(arg) do -- 可变参数列表通过arg使用
            s = s .. tostring(v) .. "111"
        end
        print(s)
        print("arg num" .. arg.n)  -- arg.n表示传递参数的个数

        for i = 1,arg.n do
            m = m ..  arg[i] .. '222'
        end
        print(m)
    end

    my_print("AAA","BBB","CCC")
    --> AAA111BBB111CCC111
    --> arg num3
    --> AAA222BBB222CCC222

命名参数
--------------------
通过将所有的参数放在一个表中，把表作为函数的唯一参数来实现命名参数的传递::

    function game(option)
        if type(option.name) ~= "string" then -- 判断值是否存在
            error("must input name")
        end

        print("name:" .. option.name ..
            " price: " .. (option.price or '') ) -- 对于不一定存在的值，可以用括号包起来先计算
    end

    game{name = "apple", price = "3.6"} -- 将所有的参数放在一个表中
    game{name = "balana"}
    --> name:apple price: 3.6
    --> name:balana price:


闭包
--------------------
学习Lua，闭包是一个不可忽略的亮点，讲到闭包前，我们需要了解几个概念：

*词法定界*
    当一个函数内部嵌套另一个函数定义时，内部的函数体可以访问外部的函数的局部变量，这种特征我们称作词法定界。

*upvalue*
    外部的局部变量（external local variable）

看一个示例::

    function newCounter()
        local i = 0
        return function()  -- anonymous function
            i = i + 1
            return i
        end
    end

    c1 = newCounter() -- 将newCounter内部的匿名函数赋给c1了。
    c2 = newCounter()
    print(c1()) --> 1    c1()执行的就是newCounter()内部的匿名函数
    print(c1()) --> 2    由于闭包的作用，c1打印的upvalue一直存在
    print(c2()) --> 1

闭包是一个函数加上它可以正确访问的upvalues。如果我们再次调用newCounter，将创建一个新的局部变量i，因此我们得到了一个作用在新的变量i上的新闭包。
技术上来讲，闭包指值而不是指函数，函数仅仅是闭包的一个原型声明；

关于闭包，这篇文章 [#]_ 讲解的很详细，对于闭包的来龙去脉，这里讲的不错 [#]_ ； 



.. [#] 闭包的概念、形式与应用 http://www.ibm.com/developerworks/cn/linux/l-cn-closure/
.. [#] lua的闭包概念理解与介绍  http://blog.csdn.net/ym012/article/details/7208750
