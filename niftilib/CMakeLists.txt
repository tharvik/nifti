if(USE_ZLIB)
	set_source_files_properties(nifti1_io.c PROPERTIES COMPILE_DEFINITIONS "HAVE_ZLIB")
endif(USE_ZLIB)

add_library(niftiio nifti1_io.c)
target_link_libraries(niftiio znz m)

target_include_directories(niftiio PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}) 

set_target_properties(niftiio PROPERTIES
	PUBLIC_HEADER "nifti1.h;nifti1_io.h"
	VERSION ${VERSION}
)

install(TARGETS niftiio
	RUNTIME DESTINATION ${INSTALL_BIN_DIR}
	LIBRARY DESTINATION ${INSTALL_LIB_DIR}
	ARCHIVE DESTINATION ${INSTALL_LIB_DIR}
	PUBLIC_HEADER DESTINATION ${INSTALL_INC_DIR}
)
