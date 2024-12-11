# Drawing

______________________________________________


```lua
-- Your Lua code here
local circle = Drawing.new("Circle")
circle.Radius = 50
circle.Color = Color3.fromRGB(255, 0, 0)
circle.Filled = true
circle.NumSides = 32
circle.Position = Vector2.new(300, 300)
circle.Transparency = 0.7
circle.Visible = true

task.wait(1)
circle:Destroy()
```
