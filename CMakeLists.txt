cmake_minimum_required(VERSION 3.8)
project(lua C)

set (CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)
set (CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set (CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)

# Configure primary executable target
add_executable(lua onelua.c)
add_library(liblua SHARED onelua.c)
set_target_properties(liblua
    PROPERTIES
        COMPILE_FLAGS "-DLUA_BUILD_AS_DLL -DMAKE_LIB"
)

# Enable generation of .map files
if (MOLLENOS)
  set_target_properties(lua liblua
    PROPERTIES
        LINK_FLAGS "-Xlinker -lldmap"
  )
endif()

# Install library headers
install(FILES luaconf.h lua.h lauxlib.h lualib.h DESTINATION include/lua)

# Add a new install target
install(TARGETS lua liblua
    ARCHIVE DESTINATION lib
    LIBRARY DESTINATION lib
    RUNTIME DESTINATION bin
)

# install the generated map file
if (MOLLENOS)
  install(FILES ${CMAKE_BINARY_DIR}/bin/liblua.map DESTINATION bin)
  install(FILES ${CMAKE_BINARY_DIR}/bin/lua.map DESTINATION bin)
endif()
