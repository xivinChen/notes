#mac 系统安装

```
# 安装
brew install lua
# 进入控制台
lua i

# 输入print("hello lua")验证安装是否正确

```

# window操作系统安装lua

```angular2
github下载网址  https://github.com/rjpcomputing/luaforwindows/releases

下载后点击下一步直到完成即可
```


# lua的基本数据类型

```angular2
nil 表示无效的值，当一个不赋值的全局变量默认是nil,当一个全局变量赋予nil,就想当于删除操作。
boolean 
string
number
table
```

#table 的基本语法

```angular2
tab1={key1="val1",key2="val2","val3"};
for k,v in pairs(tab1) do
   print(k.. "-" ..v)
end

```

