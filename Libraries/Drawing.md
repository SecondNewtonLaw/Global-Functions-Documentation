# Drawing

The Drawing library provides an interface for rendering shapes and text onto the game window. Drawing can be implemented either through LuaU or externally, using methods such as utilizing Vulkan or ImGui overlays. Additionally, there are many other external approaches available.
- External drawing libraries are preferred to have `__OBJECT` properties, for easier access. An example for this would be `__OBJECT_EXISTS`, which allows the user to easily determine whether a drawing element still exists or not.
- **LuaU** drawing libraries face issues of moderate to high lag on the client

---

## __OBJECT

`🔎 Needs Investigation`

| Identifier | Type | Description |
| -------- | ---- | ----------- |
| `_EXISTS` | boolean | Whether the drawing exists. |



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

- *Make sure your drawing can be indexed with Remove & Destroy*

---

## Drawing

```lua
drawing = Drawing.new(type)
```

### BaseDrawing

The base class of which all drawing objects inherit. Cannot be instantiated.

| Property | Type | Description |
| -------- | ---- | ----------- |
| `Visible` | boolean | Whether the drawing is visible. Defaults to `false` on some executors. |
| `ZIndex` | number | Determines the order in which a Drawing renders relative to other drawings. |
| `Transparency` | number | The opacity of the drawing (1 is opaque, 0 is transparent). |
| `Color` | Color3 | The color of the drawing. |
| `Destroy(): ()` | function | Destroys the drawing. |

### Line

Renders a line starting at `From` and ending at `To`.

| Property | Type | Description |
| -------- | ---- | ----------- |
| `From` | Vector2 | The starting point of the line. |
| `To` | Vector2 | The ending point of the line. |
| `Thickness` | number | The thickness of the line. |

### Text

Renders text at `Position`.

| Property | Type | Description |
| -------- | ---- | ----------- |
| `Text` | string | The text to render. |
| `TextBounds` | 🔒 Vector2 | The size of the text. Cannot be set. |
| `Font` | Drawing.Font | The font to use. |
| `Size` | number | The size of the text. |
| `Position` | Vector2 | The position of the text. |
| `Center` | boolean | Whether the text should be centered horizontally. |
| `Outline` | boolean | Whether the text should be outlined. |
| `OutlineColor` | Color3 | The color of the outline. |

### Image

Draws the image data to the screen. `Data` *must* be the raw image data.

| Property | Type | Description |
| -------- | ---- | ----------- |
| `Data` | string | The raw image data. |
| `Size` | Vector2 | The size of the image. |
| `Position` | Vector2 | The position of the image. |
| `Rounding` | number | The rounding of the image. |

### Circle

Draws a circle that is centered at `Position`.

This is not a perfect circle! The greater the value for `NumSides`, the more accurate the circle will be.

| Property | Type | Description |
| -------- | ---- | ----------- |
| `NumSides` | number | The number of sides of the circle. |
| `Radius` | number | The radius of the circle. |
| `Position` | Vector2 | The position of the center of the circle. |
| `Thickness` | number | If `Filled` is false, specifies the thickness of the outline. |
| `Filled` | boolean | Whether the circle should be filled. |

### Square

Draws a rectangle starting at `Position` and ending at `Position` + `Size`.

| Property | Type | Description |
| -------- | ---- | ----------- |
| `Size` | Vector2 | The size of the square. |
| `Position` | Vector2 | The position of the top-left corner of the square. |
| `Thickness` | number | If `Filled` is false, specifies the thickness of the outline. |
| `Filled` | boolean | Whether the square should be filled. |

### Quad

Draws a four-sided figure connecting to each of the four points.

| Property | Type | Description |
| -------- | ---- | ----------- |
| `PointA` | Vector2 | The first point. |
| `PointB` | Vector2 | The second point. |
| `PointC` | Vector2 | The third point. |
| `PointD` | Vector2 | The fourth point. |
| `Thickness` | number | If `Filled` is false, specifies the thickness of the outline. |
| `Filled` | boolean | Whether the quad should be filled. |

### Triangle

Draws a triangle connecting to each of the three points.

| Property | Type | Description |
| -------- | ---- | ----------- |
| `PointA` | Vector2 | The first point. |
| `PointB` | Vector2 | The second point. |
| `PointC` | Vector2 | The third point. |
| `Thickness` | number | If `Filled` is false, specifies the thickness of the outline. |
| `Filled` | boolean | Whether the triangle should be filled. |

---
