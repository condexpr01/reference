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

## logic
> 命令缓冲区: 所有调用的API最终只是在往ImDrawList数组里追加绘制图元    
> 输入: ImGuiIO结构体，当前所有输入    
> 哈希ID栈: ImGuiContext和哈希栈，每个控制都分配唯一ImGuiID    
> 布局坐标: 全局的CursorPos和CursorScreenPos，每次调用控件都会在光标位置占一块    
> 配置栈: 颜色样式字体ID前缀裁剪矩形都是通过栈管理的    

```cpp
//获取drawlist:

ImDrawList* GetWindowDrawList();//get draw list associated to the current window, to append your own drawing primitives
// Background/Foreground Draw Lists
ImDrawList* GetBackgroundDrawList(ImGuiViewport* viewport = NULL);// get background draw list for the given viewport or viewport associated to the current window. this draw list will be the first rendering one. Useful to quickly draw shapes/text behind dear imgui contents.
ImDrawList* GetForegroundDrawList(ImGuiViewport* viewport = NULL);// get foreground draw list for the given viewport or viewport associated to the current window. this draw list will be the top-most rendered one. Useful to quickly draw shapes/text over dear imgui contents.
ImDrawListSharedData* GetDrawListSharedData();// you may use this when creating your own ImDrawList instances.


// ImDrawList: 帧绘制命令构建器。你每帧调用的所有 Add* 函数，最终都转化为这里的数据。
// 后端渲染器只需要读取 CmdBuffer/VtxBuffer/IdxBuffer 即可绘制。
struct ImDrawList
{
    // ======================== 一、核心输出缓冲区（最终交给 GPU 的数据） ========================

    // 绘制命令数组。每条命令定义一个裁剪矩形(ClipRect)和纹理ID(TextureId)，
    // 并指向 VtxBuffer/IdxBuffer 中的一段范围 [ElemOffset, ElemCount)。
    ImVector<ImDrawCmd>     CmdBuffer;

    // 索引缓冲区。GPU 通过索引来复用顶点，绘制三角形。一般为 16-bit 或 32-bit。
    ImVector<ImDrawIdx>     IdxBuffer;

    // 顶点缓冲区。每个顶点包含坐标 (x,y)、UV (u,v) 和颜色 (Color)。
    ImVector<ImDrawVert>    VtxBuffer;

    // 当前 DrawList 的标志（如是否启用抗锯齿线条/填充）。
    ImDrawListFlags         Flags;

    // ======================== 二、内部运行状态（构建过程中的临时变量） ========================

    // 当前顶点缓冲区的全局索引计数（即下一个顶点的序号）。用于计算 ElemOffset。
    int                     _VtxCurrentIdx;

    // 指向 ImDrawListSharedData 的指针，包含字体、缩放、抗锯齿参数等全局共享数据。
    const ImDrawListSharedData* _Data;

    // 指向 VtxBuffer 当前可写位置的指针（高性能直接写入，无需反复调用 push_back）。
    ImDrawVert*             _VtxWritePtr;

    // 指向 IdxBuffer 当前可写位置的指针。
    ImDrawIdx*              _IdxWritePtr;

    // 当前正在构建的路径顶点缓存（用于 AddCircle, AddBezier 等复杂图形）。
    // 调用 PathLineTo 等函数时，顶点暂时存在这里，最后通过 PathStroke/Fill 转换成 VtxBuffer。
    ImVector<ImVec2>        _Path;

    // 绘制命令头信息（内部用于合并相邻同状态命令）。
    ImDrawCmdHeader         _CmdHeader;

    // 通道分割器（ChannelsSplit 功能）。用于将绘制命令分层，以便重新排序（例如先画背景层再画前景层）。
    ImDrawListSplitter      _Splitter;

    // 裁剪矩形栈。PushClipRect 压入，PopClipRect 弹出。当前栈顶决定所有 Add* 函数的绘制范围。
    ImVector<ImVec4>        _ClipRectStack;

    // 纹理 ID 栈。PushTextureID 压入，PopTextureID 弹出。默认纹理 ID 由栈顶决定，但 AddImage 可覆盖。
    ImVector<ImTextureID>   _TextureStack;

    // 回调数据缓冲区（用于 AddCallback 自定义绘制）。
    ImVector<char>          _CallbacksDataBuf;

    // 边缘缩放因子（用于字体边缘抗锯齿调整）。
    float                   _FringeScale;

    // 持有此 DrawList 的窗口名称（仅用于调试）。
    const char*             _OwnerName;

    // ======================== 三、构造/ 析构========================
    ImDrawList();
    ~ImDrawList();

    // ======================== 四、状态栈管理（Push / Pop） ========================

    // 压入裁剪矩形。后续所有绘制操作都将被限制在 (p_min, p_max) 区域内。
    // 如果 allow_intersect=true，则与当前裁剪矩形做交集；否则直接替换。
    void PushClipRect(const ImVec2& p_min, const ImVec2& p_max, bool allow_intersect = false);

    // 压入全屏裁剪矩形（通常用于重置裁剪区域）。
    void PushClipRectFullScreen();

    // 弹出裁剪矩形，恢复上一级裁剪状态。
    void PopClipRect();

    // 压入纹理 ID。后续的 AddImage 若未指定纹理，默认使用此纹理。
    void PushTexture(ImTextureID texture_id); // 注：实际常用名为 PushTextureID

    // 弹出纹理 ID。
    void PopTexture(); // 注：实际常用名为 PopTextureID

    // 获取当前裁剪矩形的左上角。
    ImVec2 GetClipRectMin() const;

    // 获取当前裁剪矩形的右下角。
    ImVec2 GetClipRectMax() const;

    // ======================== 五、高级绘制工具（Add* 系列，你主要用的部分） ========================

    // --- 直线/线段 ---
    void AddLine(const ImVec2& p1, const ImVec2& p2, ImU32 col, float thickness = 1.0f);
    void AddLineH(const ImVec2& p1, float x2, ImU32 col, float thickness = 1.0f); // 水平线快捷方式
    void AddLineV(const ImVec2& p1, float y2, ImU32 col, float thickness = 1.0f); // 垂直线快捷方式

    // --- 矩形 ---
    void AddRect(const ImVec2& p_min, const ImVec2& p_max, ImU32 col, float rounding = 0.0f, ImDrawFlags flags = 0, float thickness = 1.0f); // 描边矩形
    void AddRectFilled(const ImVec2& p_min, const ImVec2& p_max, ImU32 col, float rounding = 0.0f, ImDrawFlags flags = 0); // 填充矩形
    void AddRectFilledMultiColor(const ImVec2& p_min, const ImVec2& p_max, ImU32 col_upr_left, ImU32 col_upr_right, ImU32 col_bot_right, ImU32 col_bot_left); // 四角不同颜色的渐变填充矩形

    // --- 四边形 / 三角形 ---
    void AddQuad(const ImVec2& p1, const ImVec2& p2, const ImVec2& p3, const ImVec2& p4, ImU32 col, float thickness = 1.0f);
    void AddQuadFilled(const ImVec2& p1, const ImVec2& p2, const ImVec2& p3, const ImVec2& p4, ImU32 col);
    void AddTriangle(const ImVec2& p1, const ImVec2& p2, const ImVec2& p3, ImU32 col, float thickness = 1.0f);
    void AddTriangleFilled(const ImVec2& p1, const ImVec2& p2, const ImVec2& p3, ImU32 col);

    // --- 圆形 / 椭圆 / N边形 ---
    void AddCircle(const ImVec2& center, float radius, ImU32 col, int num_segments = 0, float thickness = 1.0f);
    void AddCircleFilled(const ImVec2& center, float radius, ImU32 col, int num_segments = 0);
    void AddNgon(const ImVec2& center, float radius, ImU32 col, int num_segments, float thickness = 1.0f); // 正多边形描边
    void AddNgonFilled(const ImVec2& center, float radius, ImU32 col, int num_segments); // 正多边形填充
    void AddEllipse(const ImVec2& center, float radius_x, float radius_y, ImU32 col, float rot = 0.0f, int num_segments = 0, float thickness = 1.0f);
    void AddEllipseFilled(const ImVec2& center, float radius_x, float radius_y, ImU32 col, float rot = 0.0f, int num_segments = 0);

    // --- 文字 ---
    void AddText(const ImVec2& pos, ImU32 col, const char* text_begin, const char* text_end = NULL);
    void AddText(const ImFont* font, float font_size, const ImVec2& pos, ImU32 col, const char* text_begin, const char* text_end = NULL, float wrap_width = 0.0f, const ImVec4* cpu_fine_clip_rect = NULL);

    // --- 贝塞尔曲线 ---
    void AddBezierCubic(const ImVec2& p1, const ImVec2& p2, const ImVec2& p3, const ImVec2& p4, ImU32 col, float thickness, int num_segments = 0); // 三次贝塞尔
    void AddBezierQuadratic(const ImVec2& p1, const ImVec2& p2, const ImVec2& p3, ImU32 col, float thickness, int num_segments = 0); // 二次贝塞尔

    // --- 多段线 / 多边形填充（通用） ---
    void AddPolyline(const ImVec2* points, int num_points, ImU32 col, ImDrawFlags flags, float thickness); // 折线
    void AddConvexPolyFilled(const ImVec2* points, int num_points, ImU32 col); // 凸多边形填充（高效）
    void AddConcavePolyFilled(const ImVec2* points, int num_points, ImU32 col); // 凹多边形填充（自动三角化，稍慢）

    // --- 图像（纹理） ---
    void AddImage(ImTextureID user_texture_id, const ImVec2& p_min, const ImVec2& p_max, const ImVec2& uv_min = ImVec2(0,0), const ImVec2& uv_max = ImVec2(1,1), ImU32 col = IM_COL32_WHITE);
    void AddImageQuad(ImTextureID user_texture_id, const ImVec2& p1, const ImVec2& p2, const ImVec2& p3, const ImVec2& p4, const ImVec2& uv1 = ImVec2(0,0), const ImVec2& uv2 = ImVec2(1,0), const ImVec2& uv3 = ImVec2(1,1), const ImVec2& uv4 = ImVec2(0,1), ImU32 col = IM_COL32_WHITE); // 透视/旋转贴图
    void AddImageRounded(ImTextureID user_texture_id, const ImVec2& p_min, const ImVec2& p_max, const ImVec2& uv_min, const ImVec2& uv_max, ImU32 col, float rounding, ImDrawFlags flags = 0); // 圆角图片

    // ======================== 六、路径系统（Path 构建 + Stroke/Fill） ========================
    // 路径用于构建复杂自定义形状。先在 _Path 中描点，最后统一渲染。

    void PathClear(); // 清空当前路径
    void PathLineTo(const ImVec2& pos); // 路径添加一个直线点
    void PathLineToMergeDuplicate(const ImVec2& pos); // 如果与最后一个点重合则不添加（去重）
    void PathFillConvex(ImU32 col); // 将当前路径作为凸多边形填充（性能最好，需确保凸）
    void PathFillConcave(ImU32 col); // 将当前路径作为凹多边形填充（自动三角化）
    void PathStroke(ImU32 col, ImDrawFlags flags, float thickness); // 将当前路径作为折线描边
    void PathArcTo(const ImVec2& center, float radius, float a_min, float a_max, int num_segments = 0); // 添加圆弧（角度制）
    void PathArcToFast(const ImVec2& center, float radius, int a_min_of_12, int a_max_of_12); // 添加圆弧（12等分快速版本，用于性能敏感场景）
    void PathEllipticalArcTo(const ImVec2& center, float radius_x, float radius_y, float rot, float a_min, float a_max, int num_segments = 0); // 椭圆弧
    void PathBezierCubicCurveTo(const ImVec2& p2, const ImVec2& p3, const ImVec2& p4, int num_segments = 0); // 三次贝塞尔曲线点
    void PathBezierQuadraticCurveTo(const ImVec2& p2, const ImVec2& p3, int num_segments = 0); // 二次贝塞尔曲线点
    void PathRect(const ImVec2& p_min, const ImVec2& p_max, float rounding = 0.0f, ImDrawFlags flags = 0); // 添加矩形路径

    // ======================== 七、底层回调与命令控制 ========================

    // 插入自定义绘制回调。允许你在 ImGui 的绘制流中插入原生图形 API（如 OpenGL 函数）代码。
    void AddCallback(ImDrawCallback callback, void* callback_data);

    // 手动添加一个空白绘制命令（一般不直接调用，由系统自动管理）。
    void AddDrawCmd();

    // ======================== 八、多通道渲染（Channels） ========================

    // 分割绘制通道。用于在交错绘制时重新排序（例如：先画所有背景层，再画所有前景层）。
    void ChannelsSplit(int channels_count);
    // 合并绘制通道，输出到 CmdBuffer。
    void ChannelsMerge();
    // 切换到指定通道（索引从 0 到 channels_count-1）。
    void ChannelsSetCurrent(int channel_index);

    // ======================== 九、原始顶点/索引写入（Primitive API） ========================
    // 跳过所有形状计算，直接向缓冲区写入裸顶点和索引。用于实现自定义着色器或特殊网格。

    void PrimReserve(int idx_count, int vtx_count); // 预分配缓冲区空间
    void PrimUnreserve(int idx_count, int vtx_count); // 回退预留空间（谨慎使用）
    void PrimRect(const ImVec2& p_min, const ImVec2& p_max, ImU32 col); // 直接写入矩形顶点
    void PrimRectUV(const ImVec2& p_min, const ImVec2& p_max, const ImVec2& uv_min, const ImVec2& uv_max, ImU32 col); // 写入带 UV 的矩形顶点
    void PrimQuadUV(const ImVec2& p1, const ImVec2& p2, const ImVec2& p3, const ImVec2& p4, const ImVec2& uv1, const ImVec2& uv2, const ImVec2& uv3, const ImVec2& uv4, ImU32 col); // 写入任意四边形的 UV 顶点
    void PrimWriteVtx(const ImVec2& pos, const ImVec2& uv, ImU32 col); // 写入单个顶点（需先 PrimReserve）
    void PrimWriteIdx(ImDrawIdx idx); // 写入单个索引（需先 PrimReserve）
    void PrimVtx(const ImVec2& pos, const ImVec2& uv, ImU32 col); // 写入顶点并自动递增索引（简易封装）

    // ======================== 十、工具与克隆 ========================

    // 克隆当前 DrawList 的输出数据（用于多线程或缓存）。
    ImDrawList* CloneOutput() const;

    // ======================== 十一、内部函数 ========================
    void _SetDrawListSharedData(ImDrawListSharedData* data);
    void _ResetForNewFrame();
    void _ClearFreeMemory();
    void _PopUnusedDrawCmd();
    void _TryMergeDrawCmds();
    void _OnChangedClipRect();
    void _OnChangedTexture();
    void _OnChangedVtxOffset();
    void _SetTexture(ImTextureID texture_id);
    int  _CalcCircleAutoSegmentCount(float radius) const;
    void _PathArcToFastEx(const ImVec2& center, float radius, int a_min_sample, int a_max_sample, int a_step);
    void _PathArcToN(const ImVec2& center, float radius, float a_min, float a_max, int num_segments);
};
```

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

## overview

```txt
//section: forward decl and basic types

//scalar data types
ImGuiID ImS8 ImU8 ImS16 ImU16 ImS32 ImU32 ImS64 ImU64

//ImDrawList, ImFontAtlas layer
ImDrawChannel ImDrawCmd ImDrawData
ImDrawList ImDrawListSharedData ImDrawListSplitter ImDrawVert
ImFont ImFontAtlas ImFontAtlasBuilder ImFontAtlasRect ImFontBaked
ImFontConfig ImFontGlyph ImFontGlyphRangesBuilder ImFontLoader
ImTextureData ImTextureRect ImColor

//ImGui layer
ImGuiContext ImGuiIO ImGuiInputTextCallbackData ImGuiKeyData
ImGuiListClipper ImGuiMultiSelectIO ImGuiOnceUponAFrame
ImGuiPayload ImGuiPlatformIO ImGuiPlatformImeData
ImGuiPlatformMonitor ImGuiSelectionBasicStorage
ImGuiSelectionExternalStorage ImGuiSelectionRequest
ImGuiSizeCallbackData ImGuiStorage ImGuiStoragePair
ImGuiStyle ImGuiTableSortSpecs ImGuiTableColumnSortSpecs
ImGuiTextBuffer ImGuiTextFilter ImGuiViewport ImGuiWindowClass

//enumrations
ImGuiDir ImGuiKey ImGuiMouseSource
ImGuiSortDirection ImGuiCol ImGuiCond
ImGuiDataType ImGuiMouseButton ImGuiMouseCursor
ImGuiStyleVar ImGuiTableBgTarget

//flags
ImDrawFlags ImDrawListFlags ImDrawTextFlags
ImFontFlags ImFontAtlasFlags ImGuiBackendFlags
ImGuiButtonFlags ImGuiChildFlags ImGuiColorEditFlags
ImGuiConfigFlags ImGuiComboFlags ImGuiDockNodeFlags
ImGuiDragDropFlags ImGuiFocusedFlags ImGuiHoveredFlags
ImGuiInputFlags ImGuiInputTextFlags ImGuiItemFlags
ImGuiKeyChord ImGuiListClipperFlags ImGuiPopupFlags
ImGuiMultiSelectFlags ImGuiSelectableFlags ImGuiSliderFlags
ImGuiTabBarFlags ImGuiTabItemFlags ImGuiTableFlags
ImGuiTableColumnFlags ImGuiTableRowFlags ImGuiTreeNodeFlags
ImGuiViewportFlags ImGuiWindowFlags

//characters types
ImWchar32 ImWchar16 ImWchar

//multi-selection item index or identifier
ImGuiSelectionUserData
ImGuiInputTextCallback ImGuiSizeCallback
ImGuiMemAllocFunc ImGuiMemFreeFunc

//vec
ImVec2 ImVec4

//section: texture id
ImTextureID ImTextureRef

//section: end-user api funcs
ImGui{
	CreateContext DestroyContext
	GetCurrentContext SetCurrentContext
	GetIO GetPlatformIO
	GetStyle
	NewFrame EndFrame
	Render
	GetDrawData
	ShowDemoWindow
	ShowMetricsWindow
	ShowDebugLogWindow
	ShowIDStackToolWindow
	ShowAboutWindow
	ShowStyleEditor
	ShowStyleSelector
	ShowFontSelector
	ShowUserGuide
	GetVersion
	StyleColorsDark StyleColorsLight StyleColorsClassic
	Begin End
	BeginChild EndChild
	IsWindowAppearing
	IsWindowCollapsed
	IsWindowFocused
	IsWindowHovered
	GetWindowDrawList
	GetWindowDpiScale
	GetWindowPos GetWindowSize
	GetWindowWidth GetWindowHeight
	GetWindowViewport
	SetNextWindowPos SetNextWindowSize
	SetNextWindowSizeConstraints
	SetNextWindowContentSize
	SetNextWindowCollapsed
	SetNextWindowFocus
	SetNextWindowScroll
	SetNextWindowBgAlpha
	SetNextWindowViewport
	SetWindowPos SetWindowSize
	SetWindowCollapsed
	SetWindowFocus
	GetScrollX GetScrollY
	SetScrollX SetScrollY
	GetScrollMaxX GetScrollMaxY
	SetScrollHereX SetScrollHereY
	SetScrollFromPosX SetScrollFromPosY
	PushFont PopFont
	GetFont GetFontSize GetFontBaked
	PushStyleColor PopStyleColor
	PushStyleVar PushStyleVarX PushStyleVarY PopStyleVar
	PushItemFlag PopItemFlag
	PushItemWidth PopItemWidth
	SetNextItemWidth CalcItemWidth
	PushTextWrapPos PopTextWrapPos
	GetFontTexUvWhitePixel
	GetColorU32
	GetStyleColorVec4
	GetCursorScreenPos SetCursorScreenPos
	GetContentRegionAvail
	GetCursorPos GetCursorPosX GetCursorPosY
	SetCursorPos SetCursorPosX SetCursorPosY
	GetCursorStartPos
	Separator
	SameLine
	NewLine
	Spacing
	Dummy
	Indent Unindent
	BeginGroup EndGroup
	AlignTextToFramePadding
	GetTextLineHeight GetTextLineHeightWithSpacing
	GetFrameHeight GetFrameHeightWithSpacing
	PushID PopID GetID
	TextUnformatted
	Text TextV
	TextColored TextColoredV
	TextDisabled TextDisabledV
	TextWrapped TextWrappedV
	LabelText LabelTextV
	BulletText BulletTextV
	SeparatorText
	Button SmallButton InvisibleButton ArrowButton
	Checkbox CheckboxFlags
	RadioButton
	ProgressBar
	Bullet
	TextLink TextLinkOpenURL
	Image ImageWithBg ImageButton
	BeginCombo EndCombo Combo
	DragFloat DragFloat2 DragFloat3 DragFloat4 DragFloatRange2
	DragInt DragInt2 DragInt3 DragInt4 DragIntRange2
	DragScalar DragScalarN
	SliderFloat SliderFloat2 SliderFloat3 SliderFloat4
	SliderAngle
	SliderInt SliderInt2 SliderInt3 SliderInt4
	SliderScalar SliderScalarN
	VSliderFloat VSliderInt VSliderScalar
	InputText InputTextMultiline InputTextWithHint
	InputFloat InputFloat2 InputFloat3 InputFloat4
	InputInt InputInt2 InputInt3 InputInt4
	InputDouble InputScalar InputScalarN
	ColorEdit3 ColorEdit4
	ColorPicker3 ColorPicker4
	ColorButton
	SetColorEditOptions
	TreeNode TreeNodeV
	TreeNodeEx TreeNodeExV
	TreePush TreePop
	GetTreeNodeToLabelSpacing
	CollapsingHeader
	SetNextItemOpen
	SetNextItemStorageID
	TreeNodeGetOpen
	Selectable
	BeginMultiSelect EndMultiSelect
	SetNextItemSelectionUserData
	IsItemToggledSelection
	BeginListBox EndListBox ListBox
	PlotLines PlotHistogram
	Value
	BeginMenuBar EndMenuBar
	BeginMainMenuBar EndMainMenuBar
	BeginMenu EndMenu
	MenuItem
	BeginTooltip EndTooltip
	SetTooltip SetTooltipV
	BeginItemTooltip SetItemTooltip SetItemTooltipV
	BeginPopup BeginPopupModal EndPopup
	OpenPopup OpenPopupOnItemClick CloseCurrentPopup
	BeginPopupContextItem BeginPopupContextWindow BeginPopupContextVoid
	IsPopupOpen
	BeginTable EndTable
	TableNextRow TableNextColumn
	TableSetColumnIndex TableSetupColumn
	TableSetupScrollFreeze
	TableHeader TableHeadersRow TableAngledHeadersRow
	TableGetSortSpecs
	TableGetColumnCount
	TableGetColumnIndex TableGetRowIndex
	TableGetColumnName
	TableGetColumnFlags
	TableSetColumnEnabled
	TableGetHoveredColumn
	TableSetBgColor
	Columns
	NextColumn
	GetColumnIndex
	GetColumnWidth SetColumnWidth
	GetColumnOffset SetColumnOffset
	GetColumnsCount
	BeginTabBar EndTabBar
	BeginTabItem EndTabItem
	TabItemButton
	SetTabItemClosed
	DockSpace DockSpaceOverViewport
	SetNextWindowDockID
	SetNextWindowClass
	GetWindowDockID
	IsWindowDocked
	LogToTTY
	LogToFile
	LogToClipboard
	LogFinish
	LogButtons
	LogText LogTextV
	BeginDragDropSource SetDragDropPayload EndDragDropSource
	BeginDragDropTarget AcceptDragDropPayload EndDragDropTarget
	GetDragDropPayload
	BeginDisabled EndDisabled
	PushClipRect PopClipRect
	SetItemDefaultFocus
	SetKeyboardFocusHere
	SetNavCursorVisible
	SetNextItemAllowOverlap
	IsItemHovered
	IsItemActive
	IsItemFocused
	IsItemClicked
	IsItemVisible
	IsItemEdited
	IsItemActivated
	IsItemDeactivated
	IsItemDeactivatedAfterEdit
	IsItemToggledOpen
	IsAnyItemHovered
	IsAnyItemActive
	IsAnyItemFocused
	GetItemID
	GetItemRectMin GetItemRectMax
	GetItemRectSize
	GetItemFlags
	GetMainViewport
	GetBackgroundDrawList GetForegroundDrawList
	IsRectVisible
	GetTime
	GetFrameCount
	GetDrawListSharedData
	GetStyleColorName
	SetStateStorage
	GetStateStorage
	CalcTextSize
	ColorConvertU32ToFloat4
	ColorConvertFloat4ToU32
	ColorConvertRGBtoHSV
	ColorConvertHSVtoRGB
	IsKeyDown
	IsKeyPressed
	IsKeyReleased
	IsKeyChordPressed
	GetKeyPressedAmount
	GetKeyName
	SetNextFrameWantCaptureKeyboard
	Shortcut
	SetNextItemShortcut
	SetItemKeyOwner
	IsMouseDown
	IsMouseClicked
	IsMouseReleased
	IsMouseDoubleClicked
	IsMouseReleasedWithDelay
	GetMouseClickedCount
	IsMouseHoveringRect
	IsMousePosValid
	IsAnyMouseDown
	GetMousePos GetMousePosOnOpeningCurrentPopup
	IsMouseDragging
	GetMouseDragDelta
	ResetMouseDragDelta
	GetMouseCursor SetMouseCursor
	SetNextFrameWantCaptureMouse
	GetClipboardText SetClipboardText
	LoadIniSettingsFromDisk LoadIniSettingsFromMemory
	SaveIniSettingsToDisk SaveIniSettingsToMemory
	DebugTextEncoding
	DebugFlashStyleColor
	DebugStartItemPicker
	DebugCheckVersionAndDataLayout
	DebugLog DebugLogV
	SetAllocatorFunctions GetAllocatorFunctions
	MemAlloc MemFree
	UpdatePlatformWindows
	RenderPlatformWindowsDefault
	DestroyPlatformWindows
	FindViewportByID FindViewportByPlatformHandle
}

//section: frags & enumerations
ImGuiWindowFlags_
ImGuiChildFlags_
ImGuiItemFlags_
ImGuiInputTextFlags_
ImGuiTreeNodeFlags_
ImGuiPopupFlags_
ImGuiSelectableFlags_
ImGuiComboFlags_
ImGuiTabBarFlags_
ImGuiTabItemFlags_
ImGuiFocusedFlags_
ImGuiHoveredFlags_
ImGuiDockNodeFlags_
ImGuiDragDropFlags_
ImGuiDataType_
ImGuiDir
ImGuiSortDirection
ImGuiKey
ImGuiInputFlags_
ImGuiConfigFlags_
ImGuiBackendFlags_
ImGuiCol_
ImGuiStyleVar_
ImGuiButtonFlags_
ImGuiColorEditFlags_
ImGuiSliderFlags_
ImGuiMouseButton_
ImGuiMouseCursor_
ImGuiMouseSource
ImGuiCond_

//section: tables flags & structures
ImGuiTableFlags_
ImGuiTableColumnFlags_
ImGuiTableRowFlags_
ImGuiTableBgTarget_
ImGuiTableSortSpecs
ImGuiTableColumnSortSpecs

//vector
ImVector

//sections: style
ImGuiStyle

//sections: IO
ImGuiKeyData
ImGuiIO

//sections: misc data structures
ImGuiInputTextCallbackData
ImGuiSizeCallbackData
ImGuiWindowClass
ImGuiPayload

//sections: helpers
ImGuiOnceUponAFrame
ImGuiTextFilter
ImGuiTextBuffer
ImGuiStoragePair
ImGuiStorage
ImGuiListClipperFlags_
ImGuiListClipper
ImColor

//sections: multi-select api
ImGuiMultiSelectFlags_
ImGuiMultiSelectIO
ImGuiSelectionRequestType
ImGuiSelectionRequest
ImGuiSelectionBasicStorage
ImGuiSelectionExternalStorage

//sections: drawing api
ImDrawIdx
ImDrawCallback
ImDrawCmd
ImDrawVert
ImDrawCmdHeader
ImDrawChannel
ImDrawListSplitter
ImDrawFlags_
ImDrawListFlags_
ImDrawList
ImDrawData

//sections: texture api
ImTextureFormat
ImTextureStatus
ImTextureRect
ImTextureData

//sections: font api
ImFontConfig
ImFontGlyph
ImFontGlyphRangesBuilder
ImFontAtlasRectId
ImFontAtlasRect
ImFontAtlasFlags_
ImFontAtlas
ImFontBaked
ImFontFlags_
ImFont

//sections: viewports
ImGuiViewportFlags_
ImGuiViewport

//sections: Imgui platformio + other platform dependent interfaces
ImGuiPlatformIO
ImGuiPlatformMonitor
ImGuiPlatformImeData
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

//定义插槽的顶点属性数据, 必须绑定GL_ARRAY_BUFFER在之前
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

