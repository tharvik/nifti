add_library(znz znzlib.c)

target_include_directories(znz INTERFACE ${CMAKE_CURRENT_SOURCE_DIR})

target_link_libraries(znz $<$<BOOL:${USE_ZLIB}>:${ZLIB_LIBRARIES}>)

set_target_properties(znz PROPERTIES
	PUBLIC_HEADER znzlib.h
	VERSION ${VERSION}
)

install(TARGETS znz
	LIBRARY DESTINATION ${INSTALL_LIB_DIR}
	ARCHIVE DESTINATION ${INSTALL_LIB_DIR}
	PUBLIC_HEADER DESTINATION ${INSTALL_INC_DIR}
)
