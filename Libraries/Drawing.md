# Drawing
________________________________________________

The Drawing library provides an interface for rendering shapes and text onto the game window. Drawing can be implemented either through LuaU or externally, using methods such as utilizing Vulkan or ImGui overlays. Additionally, there are many other external approaches available.
- Most executors that utilize external drawing libraries utilize **__OBJECT_EXISTS** for support on checking if a drawing instance exists
- **LuaU** drawing libraries face issues of moderate to high lag on the client
