# Drawing

The Drawing library provides an interface for rendering shapes and text onto the game window. Drawing can be implemented either through LuaU or externally, using methods such as utilizing Vulkan or ImGui overlays. Additionally, there are many other external approaches available.
- External drawing libraries are preferred to have `__OBJECT` properties, for easier access. An example for this would be `__OBJECT_EXISTS`, which allows the user to easily determine whether a drawing element still exists or not.
- **LuaU** drawing libraries face issues of moderate to high lag on the client
