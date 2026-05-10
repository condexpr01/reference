# <font color=#ffe211>:star: GLSL</font>
> 类c语言, glm和glsl的类型大多一样


## <font color=#ffe211>:star: 渲染流程</font>
```
VAO                             (in 顶点,  out 顶点)  ->
vertex shader                   (in patch, out patch) ->
tessellation control/evaluation (in 参数点,out 顶点)  ->
geometry shader                 (in 图元,  out 图元)  ->
rasterize                       (in 图元,  out 片元)  ->
fragment shader                 (in 片元,  out 颜色)  ->
depth test ->
blend      ->
framebuffer
```


