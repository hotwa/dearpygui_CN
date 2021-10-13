# Dearpygui-translation(Chinese)

[TOC]

## [Source Doc](https://dearpygui.readthedocs.io/en/latest/?badge=latest)

## 文档

### 渲染循环

`渲染循环`：**是一个不断的运行每一个程序中的图形控件的循环。**

画出控件时是通过利用`dpg`对象进行更新每一个状态改变的图像控件。当set_viewport_vsync打开时，DPG以你的显示器刷新率来做这件事，如果vsync被关闭，那么渲染循环将尽可能快地运行，这通常不是很理想。

如果函数的计算量很大，在渲染循环中运行python可能会大大降低应用程序的帧率。

我们也不建议在渲染循环中进行`sleep(1)`这样的操作。尽管你可以通过限制渲染循环的速度来引入一个帧率上限。

对于大多数用例来说，渲染循环不需要考虑，它完全由start_dearpygui处理。

对于更高级的用例，可以像这样访问渲染循环的全部权限。

```python
import dearpygui.dearpygui as dpg

dpg.create_context()

with dpg.window(label="Example Window"):
    dpg.add_text("Hello, world")
    dpg.add_button(label="Save")
    dpg.add_input_text(label="string", default_value="Quick brown fox")
    dpg.add_slider_float(label="float", default_value=0.273, max_value=1)

dpg.create_viewport(title='Custom Title', width=600, height=200)
dpg.setup_dearpygui()
dpg.show_viewport()

# below replaces, start_dearpygui()
while dpg.is_dearpygui_running():
    # insert here any code you would like to run in the render loop
    # you can manually stop by using stop_dearpygui()
    print("this will run every frame")
    dpg.render_dearpygui_frame()

dpg.destroy_context()
```

### 视图接口

**视图接口等于其他GUI库中的窗口。**

在Dear PyGui的例子中，我们把操作系统窗口称为视图接口，把操作系统中的窗口里面Dearpygui中展示的窗口称为窗口。

在调用`start_dearpygui`之前，你必须进行一下流程设置：

1. 创建一个视图接口（也就是是操作系统窗口），使用`create_viewport`函数
2. 派发一个视图接口，使用`setup_dearpygui`函数
3. 展示这个视图接口，使用`show_viewport`函数
4. 最后调用`start_dearpygui`函数

### 基础窗口

基础窗口填充于视图窗口之中，并可调整大小置于其他窗口的后面。

可以使用`set_primary_window`(true or false)在窗口中设置主(基本)窗口

### IO、处理函数、状态轮询

**处理函数是控件调用中对这个控件的改变做出的动作，这个动作(函数或程序)往往是封装在一个函数中。**

处理函数常常有激活和非激活状态，对应着显示或隐藏某个控件。

处理函数需要被注册。（换句话说就是这个函数要在调用这个对象之前定义）

#### 控件的处理函数的状态

- Activated
- Active
- Clicked
- Deactivated
- Deactivated After Edited
- Focus
- Hover
- Resize
- Toggled
- Visible

处理函数的特性：

1. 处理函数可以绑定至某个具体的控件
2. 处理函数可以操作多个控件，可以进行重复使用在每个控件中。

#### 全局操作（IO 输入）
