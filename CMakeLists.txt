cmake_minimum_required(VERSION 3.16)

# Enable Hot Reload for MSVC compilers if supported.
if(POLICY CMP0141)
  cmake_policy(SET CMP0141 NEW)
  set(CMAKE_MSVC_DEBUG_INFORMATION_FORMAT "$<IF:$<AND:$<C_COMPILER_ID:MSVC>,$<CXX_COMPILER_ID:MSVC>>,$<$<CONFIG:Debug,RelWithDebInfo>:EditAndContinue>,$<$<CONFIG:Debug,RelWithDebInfo>:ProgramDatabase>>")
endif()

set(IMGUI_SRCS
	"external/imgui.cpp"
	"external/imgui_demo.cpp"
	"external/imgui_draw.cpp"
	"external/imgui_impl_dx12.cpp"
	"external/imgui_impl_win32.cpp"
	"external/imgui_tables.cpp"
	"external/imgui_widgets.cpp"
	#debugging stuff
	"external/imgui.natvis"
    "external/imgui.natstepfilter"
	#std::string
	"external/imgui_stdlib.cpp"
)

project(insertProjectName)

# entry point
add_executable(${PROJECT_NAME}
	"src/main.cpp"
)

# change backend here if needed
target_precompile_headers(${PROJECT_NAME} PRIVATE
    <windows.h>
    "external/imgui.h"
    "external/imconfig.h"
    "external/imgui_impl_dx12.h"
    "external/imgui_impl_win32.h"
)

target_sources(${PROJECT_NAME} PRIVATE ${IMGUI_SRCS})

target_include_directories(${PROJECT_NAME} PRIVATE
	"include/"
	"external/"
)

target_link_libraries(${PROJECT_NAME} PRIVATE 
    d3d12
	dxgi
    d3dcompiler
)

if(CMAKE_VERSION VERSION_GREATER 3.16)
  set_property(TARGET insertProjectName PROPERTY CXX_STANDARD 23)
endif()