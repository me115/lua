.. _syntax:

基本语法
====================

赋值
--------------------
::

    a = "hello".. "world"  -- 连接字符串 , 注意，语句结尾不需要‘;’之类的结束符
    n = n + 1 
    a, b = 10,20  -->  a = 10 ; b = 20
    a,b,c = 0, 1  -->  a = 0; b = 1; c = nil  不足补nil
    a,b = 0,1,2   --> a = 0; b = 1 ;  多余的 2丢弃
    x,y = y,x     --> 交换  x,y的值


if / elseif / else
--------------------
::

    a = 5
    if a > 0 then
        print("a>0")
    elseif a < 0 then
        print("a<0")
    else
        print("a==0")
    end
    --> a>0

while循环
--------------------
::

    a,str = 1,''
    while a < 5 do
        str = str .. a
        a = a  + 1
    end
    print(str)
    --> 1234

for 循环
--------------------
**数值for循环**
::

    for i =  1,10,2 do
        print(i)
        if i >= 7 then
            break
        end
    end
    --> 1 3 5 7
第三个参数为步进值，如果为1，可省略

**范型for循环**
::

    t = {"a","b","c","e"}
    for i,v in ipairs(t) do print("i:".. i) print("v:"..v) end
    --> i:1 v:a  i:2 v:b i:3 v:c  i:4  v:e

    for line inio.lines() do
        io.write(line, '\n') 
    end

table类型需要ipairs()来构造一个迭代器；


