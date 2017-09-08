#include_directories(${NIFTI_SOURCE_DIR}/niftilib ${NIFTI_SOURCE_DIR}/znzlib ${NIFTI_SOURCE_DIR}/nifticdf)

add_executable(nifti_stats nifti_stats.c)
add_executable(nifti_tool nifti_tool.c)
add_executable(nifti1_test nifti1_test.c)

set_target_properties(nifti_tool PROPERTIES PUBLIC_HEADER nifti_tool.h)

target_link_libraries(nifti_stats niftiio nifticdf)
target_link_libraries(nifti_tool  niftiio)
target_link_libraries(nifti1_test niftiio)

install(TARGETS nifti_stats nifti_tool nifti1_test
	RUNTIME DESTINATION ${INSTALL_BIN_DIR}
	LIBRARY DESTINATION ${INSTALL_LIB_DIR}
	ARCHIVE DESTINATION ${INSTALL_LIB_DIR}
	PUBLIC_HEADER DESTINATION ${INSTALL_INC_DIR}
)

add_test(nifti1_test_help ${NIFTI_BINARY_DIR}/utils/nifti1_test)
add_test(nifti1_test_n1 ${NIFTI_BINARY_DIR}/utils/nifti1_test  -n1  ${NIFTI_SOURCE_DIR}/Testing/Data/ATestReferenceImageForReadingAndWriting.nii.gz ${NIFTI_BINARY_DIR}/utils/n1 )
add_test(nifti1_test_n2 ${NIFTI_BINARY_DIR}/utils/nifti1_test  -n2  ${NIFTI_SOURCE_DIR}/Testing/Data/ATestReferenceImageForReadingAndWriting.nii.gz ${NIFTI_BINARY_DIR}/utils/n2 )
add_test(nifti1_test_a2 ${NIFTI_BINARY_DIR}/utils/nifti1_test  -a2  ${NIFTI_SOURCE_DIR}/Testing/Data/ATestReferenceImageForReadingAndWriting.nii.gz ${NIFTI_BINARY_DIR}/utils/a2 )
add_test(nifti1_test_zn1 ${NIFTI_BINARY_DIR}/utils/nifti1_test -zn1 ${NIFTI_SOURCE_DIR}/Testing/Data/ATestReferenceImageForReadingAndWriting.nii.gz ${NIFTI_BINARY_DIR}/utils/zn1 )
add_test(nifti1_test_zn2 ${NIFTI_BINARY_DIR}/utils/nifti1_test -zn2 ${NIFTI_SOURCE_DIR}/Testing/Data/ATestReferenceImageForReadingAndWriting.nii.gz ${NIFTI_BINARY_DIR}/utils/zn2 )
add_test(nifti1_test_za2 ${NIFTI_BINARY_DIR}/utils/nifti1_test -za2 ${NIFTI_SOURCE_DIR}/Testing/Data/ATestReferenceImageForReadingAndWriting.nii.gz ${NIFTI_BINARY_DIR}/utils/za2 )

#The help screens always return 1; add_test(nifti_tool_help ${NIFTI_BINARY_DIR}/utils/nifti_tool)
add_test(nifti_tool_n1 ${NIFTI_BINARY_DIR}/utils/nifti_tool  -disp_hdr -infile ${NIFTI_BINARY_DIR}/utils/n1 )
add_test(nifti_tool_n2 ${NIFTI_BINARY_DIR}/utils/nifti_tool  -disp_hdr -infile ${NIFTI_BINARY_DIR}/utils/n2 )
add_test(nifti_tool_a2 ${NIFTI_BINARY_DIR}/utils/nifti_tool  -disp_hdr -infile ${NIFTI_BINARY_DIR}/utils/a2 )
add_test(nifti_tool_zn1 ${NIFTI_BINARY_DIR}/utils/nifti_tool -disp_hdr -infile ${NIFTI_BINARY_DIR}/utils/zn1 )
add_test(nifti_tool_zn2 ${NIFTI_BINARY_DIR}/utils/nifti_tool -disp_hdr -infile ${NIFTI_BINARY_DIR}/utils/zn2 )
add_test(nifti_tool_za2 ${NIFTI_BINARY_DIR}/utils/nifti_tool -disp_hdr -infile ${NIFTI_BINARY_DIR}/utils/za2 )
add_test(nifti_tool_diff_hdr ${NIFTI_BINARY_DIR}/utils/nifti_tool -diff_hdr -infile ${NIFTI_BINARY_DIR}/utils/zn1 ${NIFTI_BINARY_DIR}/utils/zn1 )
add_test(nifti_tool_diff_nims ${NIFTI_BINARY_DIR}/utils/nifti_tool -diff_nim -infile ${NIFTI_BINARY_DIR}/utils/zn1 ${NIFTI_BINARY_DIR}/utils/zn1 )

add_test(nifti_tool_copy_brick_list ${NIFTI_BINARY_DIR}/utils/nifti_tool -cbl -prefix cbl_zn1.nii -infile ${NIFTI_BINARY_DIR}/utils/zn1.nii.gz[0,1])
#add_test(nifti_tool_copy_collapsed_image /bin/sh ${NIFTI_SOURCE_DIR}/utils/rmthenrun cci_zn1.nii ${NIFTI_BINARY_DIR}/utils/nifti_tool -cci 2 2 2 -1 -1 -1 -1 -prefix cci_zn1.nii -infile ${NIFTI_BINARY_DIR}/utils/zn1.nii.gz )
add_test(nifti_tool_disp_ci ${NIFTI_BINARY_DIR}/utils/nifti_tool -disp_ci 2 2 2 -1 0 0 0  -infile ${NIFTI_BINARY_DIR}/utils/zn1.nii.gz )
add_test(nifti_tool_disp_ts ${NIFTI_BINARY_DIR}/utils/nifti_tool -disp_ts 2 2 2 -infile ${NIFTI_BINARY_DIR}/utils/zn1.nii.gz )
add_test(nifti_tool_strip_extras ${NIFTI_BINARY_DIR}/utils/nifti_tool -strip_extras -infile ${NIFTI_BINARY_DIR}/utils/zn1.nii.gz )

# This test needs a file that has extensions to remove
#add_test(nifti_tool_rm_ext /bin/sh ${NIFTI_SOURCE_DIR}/utils/rmthenrun rm_ext.nii ${NIFTI_BINARY_DIR}/utils/nifti_tool -rm_ext ALL -prefix rm_ext.nii -infile ${NIFTI_BINARY_DIR}/utils/zn1.nii.gz )

add_test(nifti_tool_check_hdr ${NIFTI_BINARY_DIR}/utils/nifti_tool -check_hdr -infile ${NIFTI_BINARY_DIR}/utils/za2 )
add_test(nifti_tool_check_nim ${NIFTI_BINARY_DIR}/utils/nifti_tool -check_nim -infile ${NIFTI_BINARY_DIR}/utils/za2 )

foreach(DISTRIBUTION CORREL TTEST FTEST ZSCORE CHISQ BETA BINOM GAMMA POISSON NORMAL FTEST_NONC CHISQ_NONC LOGISTIC LAPLACE UNIFORM TTEST_NONC WEIBULL CHI INVGAUSS EXTVAL PVAL LOGPVAL LOG10PVAL )
    add_test(nifti_stats_${DISTRIBUTION}_test ${NIFTI_BINARY_DIR}/utils/nifti_stats 0:4:1 ${DISTRIBUTION})
    add_test(qnifti_stats_${DISTRIBUTION}_test ${NIFTI_BINARY_DIR}/utils/nifti_stats -q 0:4:1 ${DISTRIBUTION})
    add_test(q1nifti_stats_${DISTRIBUTION}_test ${NIFTI_BINARY_DIR}/utils/nifti_stats -q 0:4:1 ${DISTRIBUTION} 1 1 1)
    add_test(dnifti_stats_${DISTRIBUTION}_test ${NIFTI_BINARY_DIR}/utils/nifti_stats -d 0:4:1 ${DISTRIBUTION})
    add_test(d1nifti_stats_${DISTRIBUTION}_test ${NIFTI_BINARY_DIR}/utils/nifti_stats -d 0:4:1 ${DISTRIBUTION} 1 1 1)
    add_test(1nifti_stats_${DISTRIBUTION}_test ${NIFTI_BINARY_DIR}/utils/nifti_stats -1 0:4:1 ${DISTRIBUTION})
    add_test(11nifti_stats_${DISTRIBUTION}_test ${NIFTI_BINARY_DIR}/utils/nifti_stats -1 0:4:1 ${DISTRIBUTION} 1 1 1)
    add_test(znifti_stats_${DISTRIBUTION}_test ${NIFTI_BINARY_DIR}/utils/nifti_stats -z 0:4:1 ${DISTRIBUTION})
    add_test(z1nifti_stats_${DISTRIBUTION}_test ${NIFTI_BINARY_DIR}/utils/nifti_stats -z 0:4:1 ${DISTRIBUTION} 1 1 1)
    add_test(hnifti_stats_${DISTRIBUTION}_test ${NIFTI_BINARY_DIR}/utils/nifti_stats -h 0:4:1 ${DISTRIBUTION})
    add_test(h1nifti_stats_${DISTRIBUTION}_test ${NIFTI_BINARY_DIR}/utils/nifti_stats -h 0:4:1 ${DISTRIBUTION} 1 1 1)
endforeach(DISTRIBUTION)

