# CMakeList.txt: CMakeProject1 的 CMake 项目，在此处包括源代码并定义
# 项目特定的逻辑。
#

# 将源代码添加到此项目的可执行文件。
add_executable (CMakeProject1 "CMakeProject1.cpp" "CMakeProject1.h")

if (CMAKE_VERSION VERSION_GREATER 3.12)
  set_property(TARGET CMakeProject1 PROPERTY CXX_STANDARD 20)
endif()


set(FFMPEG_ROOT "/home/wgj/ffmpeg6" CACHE PATH "自行编译的 FFmpeg 6.0 安装目录")

set(FFMPEG_LIBS avformat avcodec avutil swscale swresample)
foreach(lib ${FFMPEG_LIBS})
  find_library(FFMPEG_${lib}_LIBRARY NAMES ${lib}
    PATHS "${FFMPEG_ROOT}/lib" NO_DEFAULT_PATH)
  if (NOT FFMPEG_${lib}_LIBRARY)
    message(FATAL_ERROR "在 ${FFMPEG_ROOT}/lib 中找不到 lib${lib}")
  endif()
  list(APPEND FFMPEG_LIBRARIES ${FFMPEG_${lib}_LIBRARY})
endforeach()

find_package(SDL2 QUIET)
if (SDL2_FOUND AND TARGET SDL2::SDL2)
  set(SDL2_LINK SDL2::SDL2)
else()
  set(SDL2_LINK SDL2)
endif()

target_include_directories(CMakeProject1 BEFORE PRIVATE "${FFMPEG_ROOT}/include")
target_link_libraries(CMakeProject1 PRIVATE ${FFMPEG_LIBRARIES} ${SDL2_LINK})
set_target_properties(CMakeProject1 PROPERTIES BUILD_RPATH "${FFMPEG_ROOT}/lib")




# TODO: 如有需要，请添加测试并安装目标。
