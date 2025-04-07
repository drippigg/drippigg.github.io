[Return Home](/)
# 🐍 Lua Notes

## ⚙️ Mechanisms > Policies

- **Mechanisms** = *how* things are done — implementation, tools, constraints (e.g., permissions, sandboxes).
- **Policies** = *what* should or shouldn't happen — guidelines or rules.
- In practice, **mechanisms enforce policies**. A good mechanism prevents violations regardless of user behavior.

---

## 💬 Comments
```lua
-- This is a single-line comment.

--[[
    This is a 
    multi-line comment.
]]
```

---

## 📦 Data Types
```lua
local number = 5                     -- Number
local string = "this is a string"   -- String
local single = 'also valid'         -- Single quotes work too
local multi = [[multi-line string]] -- Long string
local truth, lies = true, false     -- Booleans
local nothing = nil                 -- Nil (absence of value)
```

---

## 🔧 Functions

### Basic Function
```lua
local function hello(name)
    print("Hello!", name)
end
```

### Anonymous Function
```lua
local greet = function(name)
    print("Greetings, " .. name .. "!")
end
```

### Higher-Order Functions (return other functions)
```lua
local function make_adder(x)
    return function(y)
        return x + y
    end
end

local add_five = make_adder(5)
print(add_five(3))  -- Outputs 8
```

### Multiple Return Values
```lua
local function stats()
    return 100, 200, 300
end

local a, b, c = stats()
print(a, b, c)  -- 100 200 300
```

---

## 🧰 Tables

- Lua's only compound data structure (like arrays and dictionaries in one)
- Indexing starts at 1 (not 0!)

```lua
-- Array-style table
local list = {"one", 2, false, function() print("func") end}
print(list[1]) -- "one"
list[4]()      -- calls the function

-- Dictionary-style table
local t = {
    name = "Capsule Corp",
    ["space-ship"] = "Bulma",
    [function() return "dynamic" end] = true,
}

print(t.name)                -- "Capsule Corp"
print(t["space-ship"])       -- "Bulma"
```

---

## 🔁 Control Flow

```lua
-- for loop with numeric index
for i = 1, 3 do
    print(i)
end

-- for loop using ipairs (array-style)
local names = {"Ash", "Misty", "Brock"}
for i, name in ipairs(names) do
    print(i, name)
end

-- for loop using pairs (dictionary-style)
local scores = {ash = 95, misty = 90, brock = 85}
for key, value in pairs(scores) do
    print(key, value)
end

-- if-elseif-else
local x = 10
if x < 5 then
    print("small")
elseif x < 15 then
    print("medium")
else
    print("large")
end
```

---

## 📦 Modules

- Modules are just Lua files that return a table
```lua
-- mymodule.lua
local M = {}

function M.say_hello()
    print("Hello from module!")
end

return M
```

```lua
-- main.lua
local m = require("mymodule")
m.say_hello()
```

---

## 🔡 String Shorthand

```lua
print("You can call print without parentheses when using a single string literal")  --> ok
-- But only when there's no ambiguity.
```

---

## 🧪 Colon Syntax (OOP Style)

```lua
local Dog = {}

function Dog:new(name)
    local instance = {name = name}
    setmetatable(instance, self)
    self.__index = self
    return instance
end

function Dog:speak()
    print(self.name .. " says woof!")
end

local fido = Dog:new("Fido")
fido:speak()
```

> `:` automatically passes `self` as the first parameter

---

## 🧙‍♂️ Metatables

- Metatables let you override operators or customize behavior for tables.

```lua
local Vector = {}
Vector.__index = Vector

function Vector:new(x, y)
    return setmetatable({x = x, y = y}, self)
end

function Vector.__add(a, b)
    return Vector:new(a.x + b.x, a.y + b.y)
end

local v1 = Vector:new(1, 2)
local v2 = Vector:new(3, 4)
local v3 = v1 + v2

print(v3.x, v3.y)  --> 4 6
```

### Fibonacci Example with Lazy Calculation
```lua
local fib = setmetatable({}, {
    __index = function(self, n)
        if n < 2 then return 1 end
        self[n] = self[n - 1] + self[n - 2]
        return self[n]
    end
})

print(fib[10]) -- 89
```

---

## 🔄 Miscellaneous

- Lua has no classes — tables + metatables = OOP
- Everything is dynamic: functions, variables, etc.
- Closures are first-class
- `nil` is used to delete or unset values from tables

---
