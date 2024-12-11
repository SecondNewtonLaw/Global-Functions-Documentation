# Drawing
________________________________________________

The Drawing library provides an interface for rendering shapes and text onto the game window. Drawing can be implemented either through LuaU or externally, using methods such as utilizing Vulkan or ImGui overlays. Additionally, there are many other external approaches available.
- Most executors that utilize external drawing libraries utilize **__OBJECT_EXISTS** for support on checking if a drawing instance exists
- **LuaU** drawing libraries face issues of moderate to high lag on the client

  


## Drawing.new

- Drawing.new creates a new drawing object based on the type you specify in its 1st parameter.
- The drawing types are **'Line', 'Text', 'Image', 'Circle', 'Square', 'Quad', and 'Triangle'**.

```lua
function Drawing.new(type: string): Drawing
```

## Usage Example

```lua
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
