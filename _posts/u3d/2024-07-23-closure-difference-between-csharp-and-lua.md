---
layout: post
title: c# 和 lua 闭包对比
---

# C# 和 Lua 闭包对比

## c# 在循环里创建闭包


下面的`for`循环创建10个闭包，但每次都是输出10，原因是c#的闭包捕获的是变量的引用，而`i`最终结果是10

```c#
List<Action> actions = new List<Action>();
for (int i = 0; i < 10; i++)
{
    actions.Add(() =>
    {
        Debug.Log(i); // 全部输出10
    });
}

foreach (Action action in actions)
{
    action();
}
```

修正如下，用一个临时变量把`i`值存储起来
```c#
List<Action> actions = new List<Action>();
for (int i = 0; i < 10; i++)
{
    int temp = i;
    actions.Add(() =>
    {
        Debug.Log(temp); // 输出0-9
    });
}

foreach (Action action in actions)
{
    action();
}
```

## lua在循环里创建闭包

下面lua的`for`循环能正确输出1-10，原因是lua在每次`for`循环里`i`都是一个新实例

```lua
local t = {}
for i = 1, 10 do
    local fn = function() print(i) end
    table.insert(t, fn)
end

for i = 1, 10 do
    t[i]() -- 正确输出1-10
end
```

如果改成`while`循环，`i`在外面声明，则结果不一样。
```lua
local t = {}
local i = 1
while i <= 10 do
    local fn = function() print(i) end
    table.insert(t, fn)
    i = i + 1
end

for i = 1, 10 do
    t[i]() -- 全部输出11
end
```

## 为何lua中的for如此特殊？

**Lexical Scoping**：Lua严格遵循词法定界，其中在块中定义的变量只能在该块及其嵌套块中访问。这
for循环被视为一个块，因此每次迭代都有效地拥有自己的循环变量实例。