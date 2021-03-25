cmake_minimum_required(VERSION 3.8)
project(lua C)

# setup include directories
if (MOLLENOS)
  include_directories($ENV{VALI_APPLICATION_PATH}/include)
  include_directories($ENV{VALI_DDK_PATH}/include)

  link_directories($ENV{VALI_APPLICATION_PATH}/lib)
  link_directories($ENV{VALI_DDK_PATH}/lib)
endif()


set (CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)
set (CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set (CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)

# Configure primary executable target
add_executable(lua onelua.c)
if (MOLLENOS)
  target_link_libraries(lua)
endif()

add_library(liblua SHARED onelua.c)
set_target_properties(liblua
    PROPERTIES
        COMPILE_FLAGS "-DLUA_BUILD_AS_DLL -DMAKE_LIB"
)

# Install library headers
install(FILES luaconf.h lua.h lauxlib.h lualib.h DESTINATION include/lua)

# Add a new install target
install(TARGETS lua liblua
    ARCHIVE DESTINATION lib
    LIBRARY DESTINATION lib
    RUNTIME DESTINATION bin
)
