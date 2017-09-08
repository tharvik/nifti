if(USE_ZLIB)
	set_source_files_properties(nifti2_io.c PROPERTIES COMPILE_DEFINITIONS "HAVE_ZLIB")
endif(USE_ZLIB)

option(WITH_EXAMPLES "build examples")


add_library(nifti2_io nifti2_io.c)
add_executable(nifti2_tool nifti_tool.c)

target_include_directories(nifti2_io PUBLIC
	$<TARGET_PROPERTY:niftiio,INTERFACE_INCLUDE_DIRECTORIES>
)

target_link_libraries(nifti2_io znz m)
target_link_libraries(nifti2_tool nifti2_io)

set_target_properties(nifti2_io PROPERTIES
	PUBLIC_HEADER "nifti2.h;nifti2_io.h"
	VERSION "${VERSION}"
)

install(TARGETS nifti2_io nifti2_tool
	RUNTIME DESTINATION ${INSTALL_BIN_DIR}
	LIBRARY DESTINATION ${INSTALL_LIB_DIR}
	ARCHIVE DESTINATION ${INSTALL_LIB_DIR}
	PUBLIC_HEADER DESTINATION ${INSTALL_INC_DIR}
)

if(WITH_EXAMPLES)
	add_executable(clib_02_nifti2 clib_02_nifti2.c)
	target_link_libraries(clib_02_nifti2 nifti2_io)
	install(TARGETS clib_02_nifti2 RUNTIME DESTINATION ${INSTALL_BIN_DIR})
endif(WITH_EXAMPLES)
