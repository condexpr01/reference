# SDL Platform

```cpp
//manage sdl init/quit context
class sdl_ctx_manager{
	//error status
	private:
		bool status = false;
		const char* reason = nullptr;

		//get error status methos
	public:
		bool       is_ok() noexcept{return status;}
		const char* what() noexcept{return reason;}

		//RAII
	public:
		sdl_ctx_manager() noexcept{
			if (!SDL_Init(SDL_INIT_VIDEO | SDL_INIT_EVENTS)){
				status = false;
				reason = "[sdl_ctx_manager] SDL_Init";
				return;
			}

			status = true;
		}

		~sdl_ctx_manager() noexcept{SDL_Quit();}

		//neither movable nor copyable
		sdl_ctx_manager& operator=(sdl_ctx_manager  &s) =delete;
		sdl_ctx_manager& operator=(sdl_ctx_manager &&s) =delete;
};
```

# Glad
> 一个简易的库去加载opengl api

```cpp
//glad头文件必须include在SDL_opengl.h前
//在创建GLContext后, 通过gladLoadGL加载gl库里的api
//gladLoadGL的返回值是非零的glversion或者0表示错误

//manage SDL_GLContext
class sdl_gl_ctx_manager{
	//error status
	private:
		bool status = false;
		const char* reason = nullptr;

	//get error status methos
	public:
		bool       is_ok() noexcept{return status;}
		const char* what() noexcept{return reason;}

	//res
	public:
		SDL_GLContext gl_ctx = nullptr;

	//methods
	public:
		void create(SDL_Window *window) noexcept{
			if (gl_ctx){release();}

			if (window){
				gl_ctx = SDL_GL_CreateContext(window);
			}else{
				status = false;
				reason = "[sdl_gl_ctx_manager] invalid SDL_Window*";
				return;
			}

			if (!gl_ctx){
				status = false;
				reason = "[sdl_gl_ctx_manager] invalid SDL_GLContext";
				return;
			}

			if (!gladLoadGL(SDL_GL_GetProcAddress)){
				status = false;
				reason = "[sdl_gl_ctx_manager] gladLoadGL";
				return;
			}

			status = true;
		}

		void release() noexcept{if(gl_ctx){SDL_GL_DestroyContext(gl_ctx);}}

	//RAII
	public:
		sdl_gl_ctx_manager() noexcept = default;
		sdl_gl_ctx_manager(SDL_Window *window) noexcept{create(window);}

		//movable but not copyable
		sdl_gl_ctx_manager &operator=(sdl_gl_ctx_manager  &s) = delete;
		sdl_gl_ctx_manager &operator=(sdl_gl_ctx_manager &&s) = default;

		~sdl_gl_ctx_manager() noexcept{release();}
};
```

# ImGui

## font
```cpp
//在imconfig.h中定义IMGUI_USE_WCHAR32以支持更大范围的字
#define IMGUI_USE_WCHAR32
```

## size
```cpp
//ImGui::NewFrame前设置画布大小
ImGui::GetIO().DisplaySize;
ImGui::GetIO().DisplayFramebufferScale;

//设置视口，渲染的画布大小
(*ImGui::GetMainViewport()).Size;
```

## imgui ctx
```cpp
//manage sdl3 gl3 impl and imgui ctx
class sdl3_gl3_imgui_ctx_manager{
	//error status
	private:
		bool status = false;
		const char* reason = nullptr;

	//get error status methods
	public:
		bool       is_ok() noexcept{return status;}
		const char* what() noexcept{return reason;}

	//res
	public:
		ImGuiContext *imgui_ctx = nullptr;
		ImPlotContext *implot_ctx = nullptr;
		ImPlot3DContext *implot3d_ctx = nullptr;
		ImNodesContext *imnodes_ctx = nullptr;

		ImFont* font = nullptr;
		const float fontsize = 18.f;

	//RAII
	public:
		sdl3_gl3_imgui_ctx_manager(SDL_Window *glwindow,SDL_GLContext glctx) noexcept{
			imgui_ctx = ImGui::CreateContext();
			implot_ctx = ImPlot::CreateContext();
			implot3d_ctx = ImPlot3D::CreateContext();
			imnodes_ctx = ImNodes::CreateContext();

			if (!imgui_ctx || !implot_ctx || !implot3d_ctx || !imnodes_ctx){
				status = false;
				reason = "[sdl_gl_imgui_ctx_manager]CreateContext";
				return;
			}

			if(!ImGui_ImplSDL3_InitForOpenGL(glwindow,glctx)){
				status = false;
				reason = "[sdl_gl_imgui_ctx_manager]ImGui_ImplSDL3_InitForOpenGL";
				return;
			}

			if(!ImGui_ImplOpenGL3_Init("#version 460")){
				status = false;
				reason = "[sdl_gl_imgui_ctx_manager]ImGui_ImplOpenGL3_Init";
				return;
			}

			status = true;

			//default font
			std::filesystem::path font_path{std::format(
					"{:s}/fonts/SarasaUiSC-Bold.ttf",
					SDL_GetBasePath())
			};

			ImFontConfig f{};
			f.Flags = ImFontFlags_NoLoadError;

			ImGuiIO &io = ImGui::GetIO();
			if (std::filesystem::exists(font_path)){
				font = (*io.Fonts).AddFontFromFileTTF(font_path.c_str(),fontsize,&f,nullptr);
				io.FontDefault = font;
			}

			io.IniFilename = nullptr;
			io.ConfigFlags |= ImGuiConfigFlags_DockingEnable;
			io.ConfigFlags |= ImGuiConfigFlags_ViewportsEnable;
		}

		~sdl3_gl3_imgui_ctx_manager() noexcept{
			ImGui_ImplSDL3_Shutdown();
			ImGui_ImplOpenGL3_Shutdown();
			ImGui::DestroyContext(imgui_ctx);
			ImPlot::DestroyContext(implot_ctx);
			ImPlot3D::DestroyContext(implot3d_ctx);
			ImNodes::DestroyContext(imnodes_ctx);
		}

		//neither movable nor copyable
		sdl3_gl3_imgui_ctx_manager &operator=(sdl3_gl3_imgui_ctx_manager  &s) = delete;
		sdl3_gl3_imgui_ctx_manager &operator=(sdl3_gl3_imgui_ctx_manager &&s) = delete;
};
```

# GLM

```cpp
//透视正交: perspective, ortho
//view矩阵: lookAt
//平移旋转缩放: translate, rotate, scale
//叉乘点乘: cross, dot
//弧度: radians
//轴角: angleAxis
//单位化: normalize
//type_ptr: value_ptr
```

# Opengl

## SDL_Window
```
在SDL中，使用SDL_GL_MakeCurrent决定窗口使用哪个gl上下文
SDL_GL_SwapWindow交换双缓冲，用Opengl渲染结果更新一个窗口内容

图形离屏渲染：使用offscreen驱动
SDL_SetHint(SDL_HINT_VIDEO_DRIVER,"offscreen");

无需图形离屏渲染：使用dummy驱动
SDL_SetHint(SDL_HINT_VIDEO_DRIVER,"dummy");
```

## viewport
```cpp
//NDC(normalized device coordinates)
//xyz为[-1,1]的立方体空间，xy平面原点在屏幕正中央

//设置将标准化设备坐标映射到屏幕坐标规则
void glViewport(GLint x,GLint y,GLsizei width,GLsizei height);
```

## buffer object
```cpp
//生成GLuint值名字
void glGenBuffers(GLsizei n,const GLuint * buffers)

//创建buffer objects，方便DSA(direct state access)
void glCreateBuffers(GLsizei n,GLuint *buffers);

//删除有名字的缓冲对象
void glDeleteBuffers(GLsizei n,const GLuint * buffers);

//判断是不是buffer object的名字(要求buffer object得已经创建)
GLboolean glIsBuffer(GLuint buffer);

//targets :
//GL_ARRAY_BUFFER
//GL_ATOMIC_COUNTER_BUFFER
//GL_COPY_READ_BUFFER
//GL_COPY_WRITE_BUFFER
//GL_DISPATCH_INDIRECT_BUFFER
//GL_DRAW_INDIRECT_BUFFER
//GL_ELEMENT_ARRAY_BUFFER
//GL_PIXEL_PACK_BUFFER
//GL_PIXEL_UNPACK_BUFFER
//GL_QUERY_BUFFER
//GL_SHADER_STORAGE_BUFFER
//GL_TEXTURE_BUFFER
//GL_TRANSFORM_FEEDBACK_BUFFER
//GL_UNIFORM_BUFFER
//
//在buffer名字不存在buffer object时，bind buffer会创建
void glBindBuffer(GLenum target,GLuint buffer);

//创建和初始化buffer object的数据
//target: 和glBindBuffer的一样
//size: sizeof data
//data: pointer
//
//usage: 
//GL_STREAM_DRAW, GL_STREAM_READ, GL_STREAM_COPY,
//GL_STATIC_DRAW, GL_STATIC_READ, GL_STATIC_COPY,
//GL_DYNAMIC_DRAW, GL_DYNAMIC_READ, GL_DYNAMIC_COPY
//
//comment:
//现代推荐使用ubo(GL_UNIFORM_BUFFER), ssbo(GL_SHADER_STORAGE_BUFFER)，而不是glUniform*
void glBufferData(GLenum target,GLsizeiptr size,const void * data,GLenum usage);
void glNamedBufferData(GLuint buffer,GLsizeiptr size,const void *data,GLenum usage);
//
//更新buffer object存储数据的子集
void glBufferSubData(GLenum target,GLintptr offset,GLsizeiptr size,const void * data);
void glNamedBufferSubData(GLuint buffer,GLintptr offset,GLsizeiptr size,const void *data);
//
//创建和初始化不可变存储区
//flags(bitwise or combine):
//GL_DYNAMIC_STORAGE_BIT,
//GL_MAP_READ_BIT GL_MAP_WRITE_BIT, GL_MAP_PERSISTENT_BIT, GL_MAP_COHERENT_BIT,
//GL_CLIENT_STORAGE_BIT
void glBufferStorage(GLenum target, GLsizeiptr size,const void * data,GLbitfield flags);
void glNamedBufferStorage(GLuint buffer,GLsizeiptr size,const void *data,GLbitfield flags);
//
//映射buffer object的指针
//access: GL_READ_ONLY, GL_WRITE_ONLY, GL_READ_WRITE
void *glMapBuffer(GLenum target,GLenum access);
void *glMapNamedBuffer(GLuint buffer,GLenum access);
GLboolean glUnmapBuffer(GLenum target);
GLboolean glUnmapNamedBuffer(GLuint buffer);
//
//复制buffer整个或部分的数据到另一个buffer
//目标也可以用GL_COPY_READ_BUFFER, GL_COPY_WRITE_BUFFER从而不分发gl状态
void glCopyBufferSubData(GLenum readTarget,GLenum writeTarget,GLintptr readOffset,GLintptr writeOffset,GLsizeiptr size);
void glCopyNamedBufferSubData(GLuint readBuffer,GLuint writeBuffer,GLintptr readOffset,GLintptr writeOffset,GLsizeiptr size);


//生成一个framebuffer object名字
void glGenFramebuffers(GLsizei n, GLuint *ids);

//删除framebuffer object
void glDeleteFramebuffers(GLsizei n, GLuint *framebuffers);

//绑定framebuufer到framebuffer target
//target: GL_DRAW_FRAMEBUFFER, GL_READ_FRAMEBUFFER, GL_FRAMEBUFFER
void glBindFramebuffer(GLenum target, GLuint framebuffer);

//帧拷备
//通过glBindFramebuffer指定源GL_READ_FRAMEBUFFER和目标GL_DRAW_FRAMEBUFFER
//x0,y0,x1,y1: 左上角和右下角
//mask: GL_COLOR_BUFFER_BIT, GL_DEPTH_BUFFER_BIT, GL_STENCIL_BUFFER_BIT
//filter: GL_NEAREST, GL_LINEAR
void glBlitFramebuffer(
		GLint srcX0,GLint srcY0,GLint srcX1,GLint srcY1,
		GLint dstX0,GLint dstY0,GLint dstX1,GLint dstY1,
		GLbitfield mask,GLenum filter);
//DSA写法
void glBlitNamedFramebuffer(GLuint readFramebuffer,GLuint drawFramebuffer,
		GLint srcX0,GLint srcY0,GLint srcX1,GLint srcY1,
		GLint dstX0,GLint dstY0,GLint dstX1,GLint dstY1,
		GLbitfield mask,GLenum filter);

//检查framebuffer完成状态
//return value:
//GL_FRAMEBUFFER_COMPLETE
//
//GL_FRAMEBUFFER_UNDEFINED
//GL_FRAMEBUFFER_INCOMPLETE_ATTACHMENT
//GL_FRAMEBUFFER_INCOMPLETE_MISSING_ATTACHMENT
//GL_FRAMEBUFFER_INCOMPLETE_DRAW_BUFFER
//GL_FRAMEBUFFER_INCOMPLETE_READ_BUFFER
//GL_FRAMEBUFFER_UNSUPPORTED
//GL_FRAMEBUFFER_INCOMPLETE_MULTISAMPLE
//GL_FRAMEBUFFER_INCOMPLETE_LAYER_TARGET
GLenum glCheckFramebufferStatus(GLenum target);
GLenum glCheckNamedFramebufferStatus(GLuint framebuffer, GLenum target);

//关联texture object为framebuffer object的逻辑对象
//target: GL_DRAW_FRAMEBUFFER, GL_READ_FRAMEBUFFER, GL_FRAMEBUFFER
//
//attachment: GL_COLOR_ATTACHMENTi, GL_DEPTH_ATTACHMENT, GL_STENCIL_ATTACHMENT, GL_DEPTH_STENCIL_ATTACHMENT
//附件的GL_COLOR_ATTACHMENTi, i in [0,GL_MAX_COLOR_ATTACHMENTS - 1]
//
//level: mipmap level
void glFramebufferTexture(GLenum target, GLenum attachment, GLuint texture, GLint level);
void glFramebufferTexture1D(GLenum target,GLenum attachment,GLenum textarget,GLuint texture,GLint level);
void glFramebufferTexture2D(GLenum target,GLenum attachment,GLenum textarget,GLuint texture,GLint level);
void glFramebufferTexture3D(GLenum target,GLenum attachment,GLenum textarget,GLuint texture,GLint level,GLint layer);
void glNamedFramebufferTexture(GLuint framebuffer,GLenum attachment,GLuint texture,GLint level);


//render buffer objects
void glGenRenderbuffers(GLsizei n, GLuint *renderbuffers);
void glDeleteRenderbuffers(GLsizei n, GLuint *renderbuffers);

//建立renderbuffer object的图像的数据存储
//target: GL_RENDERBUFFER
//
//internalformat:
//GL_RGBA4, GL_RGB5_A1, GL_RGB565,
//GL_RGB10_A2, GL_RGB10_A2UI, GL_R11F_G11F_B10F
//GL_SRGB8_ALPHA8,
//GL_DEPTH_COMPONENT16, GL_DEPTH_COMPONENT24, GL_DEPTH_COMPONENT32F
//GL_DEPTH24_STENCIL8, GL_DEPTH32F_STENCIL8
//GL_STENCIL_INDEX8,
//...
void glRenderbufferStorage(GLenum target, GLenum internalformat, GLsizei width, GLsizei height);
void glNamedRenderbufferStorage(GLuint renderbuffer, GLenum internalformat, GLsizei width, GLsizei height);

//关联renderbuffer为framebuffer object的逻辑buffer
//target: GL_DRAW_FRAMEBUFFER, GL_READ_FRAMEBUFFER, GL_FRAMEBUFFER
//attachment: GL_COLOR_ATTACHMENTi, GL_DEPTH_ATTACHMENT, GL_STENCIL_ATTACHMENT, GL_DEPTH_STENCIL_ATTACHMENT
//renderbuffertarget: GL_RENDERBUFFER
void glFramebufferRenderbuffer(GLenum target,GLenum attachment,GLenum renderbuffertarget,GLuint renderbuffer);
void glNamedFramebufferRenderbuffer(GLuint framebuffer,GLenum attachment,GLenum renderbuffertarget,GLuint renderbuffer);

//指定光栅化点的直径
void glPointSize(GLfloat size);
```

## array vertex
```cpp
//生成vertex array object名字
void glGenVertexArrays(GLsizei n,GLuint *arrays);

//删除vertex array objects
void glDeleteVertexArrays(GLsizei n,const GLuint *arrays);

//绑定vertex array, 不存在时创建
void glBindVertexArray(GLuint array);

//判断是否是vertex array object
GLboolean glIsVertexArray(GLuint array);

//启用或禁用插槽vertex attribute array
void glEnableVertexAttribArray(GLuint index);
void glDisableVertexAttribArray(GLuint index);
void glEnableVertexArrayAttrib(GLuint vaobj,GLuint index);
void glDisableVertexArrayAttrib(GLuint vaobj,GLuint index);

//glVertexAttrib.*:
//指定插槽一个固定的值

//定义插槽的顶点属性数据
//index: 要被修改的插槽索引
//size: [1-4]
//
//type: 每个成分的类型
//GL_BYTE, GL_UNSIGNED_BYTE, GL_SHORT, GL_UNSIGNED_SHORT, GL_INT, GL_UNSIGNED_INT
//GL_HALF_FLOAT, GL_FLOAT, GL_DOUBLE, GL_FIXED,
//GL_INT_2_10_10_10_REV, GL_UNSIGNED_INT_2_10_10_10_REV ,GL_UNSIGNED_INT_10F_11F_11F_REV
//
//stride: 步长
//pointer: 一个指针地址表示的偏移值
void glVertexAttribPointer(GLuint index,GLint size,GLenum type,GLboolean normalized,GLsizei stride,const void * pointer);
void glVertexAttribIPointer(GLuint index,GLint size,GLenum type,GLboolean normalized,GLsizei stride,const void * pointer);
void glVertexAttribLPointer(GLuint index,GLint size,GLenum type,GLboolean normalized,GLsizei stride,const void * pointer);

//mode:
//GL_POINTS, GL_LINE_STRIP, GL_LINE_LOOP, GL_LINES, GL_LINE_STRIP_ADJACENCY, GL_LINES_ADJACENCY,
//GL_TRIANGLE_STRIP, GL_TRIANGLE_FAN, GL_TRIANGLES, GL_TRIANGLE_STRIP_ADJACENCY, GL_TRIANGLES_ADJACENCY
//GL_PATCHES
//
//first: first index in the enabled arrays
//count: the number of indices to be rendered.
void glDrawArrays(GLenum mode,GLint first,GLsizei count);



//mode:
//GL_POINTS, GL_LINE_STRIP, GL_LINE_LOOP, GL_LINES, GL_LINE_STRIP_ADJACENCY, GL_LINES_ADJACENCY,
//GL_TRIANGLE_STRIP, GL_TRIANGLE_FAN, GL_TRIANGLES, GL_TRIANGLE_STRIP_ADJACENCY, GL_TRIANGLES_ADJACENCY
//GL_PATCHES
//
//count: number of elements to be rendered
//type: GL_UNSIGNED_BYTE, GL_UNSIGNED_SHORT, GL_UNSIGNED_INT
//indices: byte offset(cast to pointer type) into the buffer bound to GL_ELEMENT_ARRAY_BUFFER
void glDrawElements(GLenum mode,GLsizei count,GLenum type,const void * indices);

//多边形rasterization控制
//face: Must be GL_FRONT_AND_BACK for front- and back-facing polygons.
//mode: GL_POINT, GL_LINE, GL_FILL
void glPolygonMode(GLenum face,GLenum mode);
```

## shader program

```cpp
//shader type:
//GL_COMPUTE_SHADER, GL_VERTEX_SHADER, GL_TESS_CONTROL_SHADER, GL_TESS_EVALUATION_SHADER, GL_GEOMETRY_SHADER, GL_FRAGMENT_SHADER
GLuint glCreateShader(GLenum shaderType);

//删除shader object, 或留下删除的flag直到没有联系上的program object
void glDeleteShader(GLuint shader);

//判断是不是shader
GLboolean glIsShader(GLuint shader);

//替换shader object里的源码
void glShaderSource(GLuint shader,GLsizei count,const GLchar **string,const GLint *length);

//编译shader
void glCompileShader(GLuint shader);

//pname: GL_SHADER_TYPE, GL_DELETE_STATUS, GL_COMPILE_STATUS, GL_INFO_LOG_LENGTH, GL_SHADER_SOURCE_LENGTH
void glGetShaderiv(GLuint shader,GLenum pname,GLint *params);

//返回shader object的信息日志
void glGetShaderInfoLog(GLuint shader,GLsizei maxLength,GLsizei *length,GLchar *infoLog);

//创建program object
GLuint glCreateProgram(void);

//删除program object
void glDeleteProgram(GLuint program);

//关联program object和shader object
void glAttachShader(GLuint program,GLuint shader);

//分离program object和shader object
void glDetachShader(GLuint program,GLuint shader);

//链接program object，生成可执行程序
void glLinkProgram(GLuint program);

//安装program object成为当前渲染状态的一部分
void glUseProgram(GLuint program);

//pname: 
//GL_DELETE_STATUS, GL_LINK_STATUS, GL_VALIDATE_STATUS, GL_INFO_LOG_LENGTH,
//GL_ATTACHED_SHADERS, GL_ACTIVE_ATOMIC_COUNTER_BUFFERS, GL_ACTIVE_ATTRIBUTES,
//GL_ACTIVE_ATTRIBUTE_MAX_LENGTH, GL_ACTIVE_UNIFORMS, GL_ACTIVE_UNIFORM_BLOCKS,
//GL_ACTIVE_UNIFORM_BLOCK_MAX_NAME_LENGTH, GL_ACTIVE_UNIFORM_MAX_LENGTH, GL_COMPUTE_WORK_GROUP_SIZE,
//GL_PROGRAM_BINARY_LENGTH, GL_TRANSFORM_FEEDBACK_BUFFER_MODE,
//GL_TRANSFORM_FEEDBACK_VARYINGS, GL_TRANSFORM_FEEDBACK_VARYING_MAX_LENGTH,
//GL_GEOMETRY_VERTICES_OUT, GL_GEOMETRY_INPUT_TYPE, GL_GEOMETRY_OUTPUT_TYPE.
void glGetProgramiv(GLuint program,GLenum pname,GLint *params);

//返回program object日志信息
void glGetProgramInfoLog(GLuint program,GLsizei maxLength,GLsizei *length,GLchar *infoLog);

//name: uniform name
GLint glGetUniformLocation(GLuint program,const GLchar *name);

//设置uniform的值
//void glProgramUniform.*
```

## clear
```cpp
//mask: GL_COLOR_BUFFER_BIT, GL_DEPTH_BUFFER_BIT, GL_STENCIL_BUFFER_BIT
void glClear(GLbitfield mask);

//rgba分别是浮点[0.f,1.f]
void glClearColor(GLfloat red,GLfloat green,GLfloat blue,GLfloat alpha);

//depth buffer清除值[0,1]
void glClearDepth(GLdouble depth);
void glClearDepthf(GLfloat depth);

//stencil buffer清除值
void glClearStencil(GLint s);
```

## texture

> 双线性: uv两轴上找最近的2x2线性插值    
> 三线性: 双线性的基础上再加上对最近两个mipmap间线性插值    
> mipmap: 等比例的缩小到1x1的所有层级    
> ripmap：不等比例的缩小到1xh,wx1,1x1的所有层级    
> 各向异性: 通过在mipmap上沿着纹理拉伸方向（长轴）进行多次非均匀采样来模拟,Nx=长轴采样点/短轴=N/1    
> EWA: 椭圆加权平均，最精确最好的开销最大的算法    

> aliasing: 发生在低频采高频，出现幅度重叠，高频和低频交叉    
> antialiasing: 先模糊，过滤高频，再采样, 理论教科书方式的    
> MSAA(multisample anti-aliasing): 用更多的采样点反走样，最终颜色按覆盖率来算    
> FXAA(fast aproximate aa): 走样后的后期处理，检测边缘的锯齿然后替换    
> TAA(termporal aa): 走样后的后期处理，上一帧的值来修正当前帧    
> DLSS(deep learning super sampling): 大力水手，模型计算推导猜出来像素    
> FSR(fidelityFX super resolution): 算法计算推导猜出来像素    

```cpp
//生成纹理名字
void glGenTextures(GLsizei n, GLuint *textures);

//删除texture objects
void glDeleteTextures(GLsizei n, const GLuint *textures);

//target:
//GL_TEXTURE_1D, GL_TEXTURE_2D, GL_TEXTURE_3D,
//GL_TEXTURE_1D_ARRAY, GL_TEXTURE_2D_ARRAY,
//GL_TEXTURE_RECTANGLE, GL_TEXTURE_CUBE_MAP,
//GL_TEXTURE_CUBE_MAP_ARRAY, GL_TEXTURE_BUFFER,
//GL_TEXTURE_2D_MULTISAMPLE, GL_TEXTURE_2D_MULTISAMPLE_ARRAY
void glBindTexture(GLenum target, GLuint texture);

//设置纹理参数
//target:
//GL_TEXTURE_1D, GL_TEXTURE_1D_ARRAY,
//GL_TEXTURE_2D, GL_TEXTURE_2D_ARRAY, GL_TEXTURE_2D_MULTISAMPLE, GL_TEXTURE_2D_MULTISAMPLE_ARRAY,
//GL_TEXTURE_3D,
//GL_TEXTURE_CUBE_MAP, GL_TEXTURE_CUBE_MAP_ARRAY, GL_TEXTURE_RECTANGLE
//GL_TEXTURE_BORDER_COLOR, GL_TEXTURE_SWIZZLE_RGBA.
//
//pname:
//GL_DEPTH_STENCIL_TEXTURE_MODE, GL_TEXTURE_BASE_LEVEL, GL_TEXTURE_COMPARE_FUNC,
//GL_TEXTURE_COMPARE_MODE, GL_TEXTURE_LOD_BIAS,
//GL_TEXTURE_MIN_FILTER, GL_TEXTURE_MAG_FILTER,
//GL_TEXTURE_MIN_LOD, GL_TEXTURE_MAX_LOD,
//GL_TEXTURE_MAX_LEVEL,
//GL_TEXTURE_SWIZZLE_R, GL_TEXTURE_SWIZZLE_G, GL_TEXTURE_SWIZZLE_B, GL_TEXTURE_SWIZZLE_A,
//GL_TEXTURE_WRAP_S, GL_TEXTURE_WRAP_T, GL_TEXTURE_WRAP_R
//
//see man doc for params
//void glTexParameter.*


//为指定的texture object生成mipmap
//target:
//GL_TEXTURE_1D, GL_TEXTURE_2D, GL_TEXTURE_3D,
//GL_TEXTURE_1D_ARRAY, GL_TEXTURE_2D_ARRAY,
//GL_TEXTURE_CUBE_MAP, GL_TEXTURE_CUBE_MAP_ARRAY
void glGenerateMipmap(GLenum target);
void glGenerateTextureMipmap(GLuint texture); //DSA

//指定N维的纹理图像
//glTexImagenD arguments:
//level: mipmap level (0 for base image, 1 for first mipmap level, etc.)
//internalformat: how to store the texture internally with optional precision (GL_RGBA,GL_RGBA8,etc.)
//width, height, depth: dimensions of the texture
//border: must be 0, historical
//format: format of the pixel, only data sequence without precision (GL_RGBA GL_BGRA etc.)
//type: data type of the pixel data (GL_UNSIGNED_BYTE, GL_FLOAT, etc.)
//pixels: pointer to the image data in memory
//
//void glTexImage.*

//激活纹理单元, 之后glBindTexture会绑定上这个纹理单元
//texture: GL_TEXTUREi, i in [0,GL_MAX_COMBINED_TEXTURE_IMAGE_UNITS-1]
//gl保证[0,80]可用
void glActiveTexture(GLenum texture);
```

## camera

```cpp
class camera{

public:
	glm::vec3 pos{0,0,2};
	glm::vec3 center_pos = pos + glm::vec3{0,0,-1};
	glm::vec3 up_pos     = pos + glm::vec3{0,1,0};

	glm::mat4 look_at(){
		return glm::lookAt(pos,center_pos,up_pos-pos);
	}

	glm::mat4 look_at(glm::vec3 eye,glm::vec3 center,glm::vec3 up){
		pos = eye;
		center_pos = center;
		up_pos = eye + up;

		return look_at();
	}

	//自由视角更新姿态，但自动roll以保持right向量平行于xz平面
	void auto_roll_rotate(float dyaw, float dpitch) {

		glm::vec3 up{up_pos - pos};
		glm::vec3 front{center_pos - pos};
		glm::vec3 right{};

		//yaw
		if (dyaw){
			glm::quat qyaw = glm::angleAxis(glm::radians(-dyaw),up);

			//yaw运算后,更新front center_pos right
			front = qyaw * front;
			center_pos = front + pos;

			right = glm::cross(up, front);
		}

		//pitch
		if (dpitch){
			right = glm::cross(up, front);
			glm::quat qpitch = glm::angleAxis(glm::radians(-dpitch),right);

			//pitch运算后,更新front,up center_pos,up_pos right
			up = qpitch * up;
			front = qpitch * front;
			up_pos = up + pos;
			center_pos = front + pos;

			right = glm::cross(up, front);
		}

		//auto roll
		if (right.y != 0.f){
			//线面角 droll>=0
			float droll = glm::asin(glm::abs(glm::dot(right,glm::vec3{0,1,0}) / glm::length(right)));

			//调整方向, up.y决定上下, xz平面上往顺时针(负方向)，下往逆时针(正方向)
			if((right.y > 0.f && up.y > 0.f) || (right.y < 0.f && up.y < 0.f)){droll = -droll;}
			
			glm::quat qroll = glm::angleAxis(droll,front);

			//roll运算后,更新up up_pos
			up = qroll * up;
			up_pos = up + pos;
		}

	}

	//自由更新姿态
	//顺序: dyaw -> dpitch -> droll
	//使用局部的坐标系, 无万向锁
	//正：顺时针,负:逆时针
	void rotate(float dyaw, float dpitch, float droll) {

		glm::vec3 up{up_pos - pos};
		glm::vec3 front{center_pos - pos};

		//yaw
		if (dyaw){
			glm::quat qyaw = glm::angleAxis(glm::radians(-dyaw),up);

			//yaw运算后,更新front center_pos
			front = qyaw * front;
			center_pos = front + pos;
		}

		//pitch
		if (dpitch){
			glm::vec3 right = glm::cross(up, front);
			glm::quat qpitch = glm::angleAxis(glm::radians(-dpitch),right);

			//pitch运算后,更新front,up center_pos,up_pos
			up = qpitch * up;
			front = qpitch * front;
			up_pos = up + pos;
			center_pos = front + pos;
		}

		//roll
		if (droll){
			glm::quat qroll = glm::angleAxis(glm::radians(-droll),front);

			//roll运算后,更新up up_pos
			up = qroll * up;
			up_pos = up + pos;
		}

	}

	//limit in (-pi/2,pi/2)
	void limit_rotate(float dyaw, float dpitch, float limit_angle) {

		glm::vec3 up{up_pos - pos};
		glm::vec3 front{center_pos - pos};
		glm::vec3 right{};

		const float limit = 0.f;

		//yaw
		if (dyaw){
			glm::quat qyaw = glm::angleAxis(glm::radians(-dyaw),up);

			//yaw运算后,更新front center_pos right
			front = qyaw * front;
			center_pos = front + pos;

			right = glm::cross(up, front);
		}

		//pitch
		if (dpitch){
			right = glm::cross(up, front);
			glm::quat qpitch = glm::angleAxis(glm::radians(-dpitch),right);

			//pitch运算后,更新front,up center_pos,up_pos right

			glm::vec3 update_up = qpitch * up;

			//up.y wont be under xz plane
			if (update_up.y > limit){
				up = update_up;
				front = qpitch * front;
				up_pos = up + pos;
				center_pos = front + pos;

				right = glm::cross(up, front);
			}

		}

		//auto roll
		if (right.y != 0.f){
			//线面角 droll>=0
			float droll = glm::asin(glm::abs(glm::dot(right,glm::vec3{0,1,0}) / glm::length(right)));

			//调整方向, up.y决定上下, xz平面上往顺时针(负方向)，下往逆时针(正方向)
			if((right.y > 0.f && up.y > 0.f) || (right.y < 0.f && up.y < 0.f)){droll = -droll;}
			
			glm::quat qroll = glm::angleAxis(droll,front);

			//roll运算后,更新up up_pos
			up = qroll * up;
			up_pos = up + pos;
		}

	}


	void translate(float x, float y,float nagtive_z){
		glm::vec3 front = glm::normalize(center_pos - pos);
		glm::vec3 up    = glm::normalize(up_pos - pos);
		glm::vec3 right = glm::normalize(glm::cross(up,-front));

		glm::mat4 translate = glm::translate(glm::mat4(1.f),x * right + y * up + nagtive_z * front);

		center_pos = translate * glm::vec4{center_pos,1.f};
		up_pos = translate * glm::vec4{up_pos,1.f};
		pos = translate * glm::vec4{pos,1.f};
	}

};
```

## gl capabilities

```cpp
//启用或禁用服务器端的gl能力
//cap:
//GL_BLEND
//GL_CLIP_DISTANCEi
//GL_COLOR_LOGIC_OP
//GL_CULL_FACE
//GL-DEBUG_OUTPUT, GL_DEBUG_OUTPUT_SYNCHRONOUS
//GL_DEPTH_CLAMP
//GL_DEPTH_TEST
//GL_DITHER
//GL_FRAMEBUFFER_SRGB
//GL_LINE_SMOOTH
//GL_MULTISAMPLE
//GL_POLYGON_OFFSET_FILL
//GL_POLYGON_OFFSET_LINE
//GL_POLYGON_OFFSET_POINT
//GL_POLYGON_SMOOTH
//GL_PRIMITIVE_RESTART, GL_PRIMITIVE_RESTART_FIXED_INDEX
//GL_RASTERIZER_DISCARD
//GL_SAMPLE_ALPHA_TO_COVERAGE
//GL_SAMPLE_ALPHA_TO_ONE
//GL_SAMPLE_COVERAGE
//GL_SAMPLE_SHADING
//GL_SAMPLE_MASK
//GL_SCISSOR_TEST
//GL_STENCIL_TEST
//GL_TEXTURE_CUBE_MAP_SEAMLESS
//GL_PROGRAM_POINT_SIZE
void glEnable(GLenum cap);
void glDisable(GLenum cap);
void glEnablei(GLenum cap, GLuint index);
void glDisablei(GLenum cap, GLuint index);

//启用或禁用对depth buffer的写入
void glDepthMask(GLboolean flag);

//func(The initial value is GL_LESS):
//GL_NEVER, GL_LESS, GL_EQUAL,
//GL_LEQUAL, GL_GREATER, GL_NOTEQUAL,
//GL_GEQUAL, GL_ALWAYS 
void glDepthFunc(GLenum func);

//位与控制模板缓冲区的字节的哪些位是否可写
void glStencilMask(GLuint mask);

//控制什么时候通过模板测试
//func: GL_NEVER, 
//GL_LESS, GL_LEQUAL,
//GL_GREATER, GL_GEQUAL,
//GL_EQUAL, GL_NOTEQUAL,
//GL_ALWAYS(The initial value is GL_ALWAYS)
//
//ref: 模板测试的参考值[0,2^n-1], n为stencil buffer的元素的位数
//mask: 测试结束后的一个遮罩值, 会和参考值以及模板值位与
void glStencilFunc(GLenum func, GLint ref, GLuint mask);

//设置模板测试行为：GL_KEEP, GL_ZERO, GL_REPLACE, GL_INCR, GL_INCR_WRAP,
//GL_DECR, GL_DECR_WRAP, and GL_INVERT.
//The initial value is GL_KEEP
//
//sfail: 模板比较失败时
//dpfail: 模板比较通过，深度比较失败时
//dpppass: 模板比较，深度比较都通过时
void glStencilOp(GLenum sfail, GLenum dpfail, GLenum dppass);

//指定像素混合算法
//GL_ZERO, GL_ONE,
//GL_SRC_COLOR, GL_ONE_MINUS_SRC_COLOR,
//GL_DST_COLOR, GL_ONE_MINUS_DST_COLOR,
//GL_SRC_ALPHA, GL_ONE_MINUS_SRC_ALPHA,
//GL_DST_ALPHA, GL_ONE_MINUS_DST_ALPHA.
//GL_CONSTANT_COLOR, GL_ONE_MINUS_CONSTANT_COLOR,
//GL_CONSTANT_ALPHA, GL_ONE_MINUS_CONSTANT_ALPHA.
void glBlendFunc(GLenum sfactor, GLenum dfactor);
void glBlendFunci(GLuint buf, GLenum sfactor, GLenum dfactor);

//OIT: order-independent transparency，一种实现是按距离加权

//指定正面clockwise,counterclockwise
//mode: GL_CW, GL_CCW
void glFrontFace(GLenum mode);

//面剔除
//mode: GL_FRONT, GL_BACK, GL_FRONT_AND_BACK
void glCullFace(GLenum mode);

//获取uniform block index
GLuint glGetUniformBlockIndex(GLuint program,const GLchar *uniformBlockName);

//给一个绑定点给uniform块
//uniformBlockBinding: 绑定点
void glUniformBlockBinding(GLuint program,GLuint uniformBlockIndex,GLuint uniformBlockBinding);

//绑定bo到一个有绑定点索引的buffer target
//target: GL_ATOMIC_COUNTER_BUFFER, GL_TRANSFORM_FEEDBACK_BUFFER, GL_UNIFORM_BUFFER, GL_SHADER_STORAGE_BUFFER
//index: 在哪个绑定点
//buffer: buffer object
void glBindBufferBase(GLenum target,GLuint index,GLuint buffer);
void glBindBufferRange(GLenum target,GLuint index,GLuint buffer,GLintptr offset,GLsizeiptr size);


//指定重置图元的索引
void glPrimitiveRestartIndex(GLuint index);
```

## ray

> 法线变换：法线不能translate, 不等比例缩放scale, 但是可以rotate,    
> MVP中的Model矩阵需要变换,    
> 用类型转换到mat3去除translate,    
> 用逆转置处理不等比例缩放    
> $normal = transpose(inverse(mat3(M))) * origin_normal$    

> 阴影：用光的视角创建深度贴图，光看不到的为阴影    

> 法线贴图：保存法线信息的贴图    
> 切线空间：TBN三个向量表示的空间     
> 视差贴图：使用高度贴图和纹理计算新的uv坐标    

> 人眼是感知指数的，在specular求值时调整pow取得更好的效果    

> Phong-shading: 环境光照ambient+漫反射diffuse+镜面反射specular    
> Blinn-Phong: 用半程向量来代替反射向量的Phong光照    

> HDR：计算更大范围的亮度，再色调映射到[0,1]    
> Bloom泛光：亮度超过特定值的区域提取，模糊，变回原尺寸混合    
> SSAO(Screen-Space Ambient Occlusion)：通过深度缓冲计算遮挡的占比计算遮蔽系数使区域变暗    

> 正向着色：以O(m*n)的时间计算所有光照的影响    
> 延迟着色：先存各种信息到gBuffer里，再对gBuffer里的数据计算光照O(m+n)    

## z-fighting

> 缓解z-fighting原理: 使用32位的浮点，反向z-buffer(ieee754浮点的状态在0处分布最多)，0表示最深，1表示最浅    
> 缓解z-fighting: 将DepthFunc设置为GREATER, ClearDepth设置为0.f，perspective矩阵的znear和zfar互换    

## gamma
> vin, vout为归一化的sRGB分量    
> $ vout = vin ^ {gamma} $    

## read pixels

```cpp
//读取当前绑定的framebuffer的像素到data里
//format: 
//GL_STENCIL_INDEX, GL_DEPTH_COMPONENT, GL_DEPTH_STENCIL,
//GL_RED, GL_GREEN, GL_BLUE,
//GL_RGB, GL_BGR, GL_RGBA, GL_BGRA
//
//type:
//GL_UNSIGNED_BYTE, GL_BYTE,
//GL_UNSIGNED_SHORT, GL_SHORT,
//GL_UNSIGNED_INT, GL_INT,
//GL_HALF_FLOAT, GL_FLOAT,
//GL_UNSIGNED_BYTE_3_3_2, GL_UNSIGNED_BYTE_2_3_3_REV,
//GL_UNSIGNED_SHORT_5_6_5, GL_UNSIGNED_SHORT_5_6_5_REV,
//GL_UNSIGNED_SHORT_4_4_4_4, GL_UNSIGNED_SHORT_4_4_4_4_REV,
//GL_UNSIGNED_SHORT_5_5_5_1, GL_UNSIGNED_SHORT_1_5_5_5_REV,
//GL_UNSIGNED_INT_8_8_8_8, GL_UNSIGNED_INT_8_8_8_8_REV,
//GL_UNSIGNED_INT_10_10_10_2, GL_UNSIGNED_INT_2_10_10_10_REV,
//GL_UNSIGNED_INT_24_8, GL_UNSIGNED_INT_10F_11F_11F_REV,
//GL_UNSIGNED_INT_5_9_9_9_REV, GL_FLOAT_32_UNSIGNED_INT_24_8_REV
void glReadPixels(GLint x,GLint y,GLsizei width,GLsizei height,GLenum format,GLenum type,void * data);
void glReadnPixels(GLint x,GLint y,GLsizei width,GLsizei height,GLenum format,GLenum type,GLsizei bufSize,void *data);
```

