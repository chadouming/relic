string(TOLOWER ${ARITH} ARITH_PATH)
if (EXISTS "${CMAKE_CURRENT_SOURCE_DIR}/low/${ARITH_PATH}/CMakeLists.txt")
	include("${CMAKE_CURRENT_SOURCE_DIR}/low/${ARITH_PATH}/CMakeLists.txt")
endif(EXISTS "${CMAKE_CURRENT_SOURCE_DIR}/low/${ARITH_PATH}/CMakeLists.txt")

if (NOT INHERIT)
	set(INHERIT "easy")
endif(NOT INHERIT)
string(TOLOWER ${INHERIT} INHERIT_PATH)

set(CORE_SRCS relic_err.c relic_core.c relic_conf.c relic_util.c)

if (ARCH)
	string(TOLOWER ${ARCH} ARCH_PATH)
	if (EXISTS "${CMAKE_CURRENT_SOURCE_DIR}/arch/relic_arch_${ARCH_PATH}.c")
		list(APPEND CORE_SRCS "arch/relic_arch_${ARCH_PATH}.c")
	endif(EXISTS "${CMAKE_CURRENT_SOURCE_DIR}/arch/relic_arch_${ARCH_PATH}.c")
else(ARCH)
	list(APPEND CORE_SRCS "arch/relic_arch_none.c")
endif(ARCH)

string(TOLOWER ${RAND} RAND_PATH)
if (EXISTS "${CMAKE_CURRENT_SOURCE_DIR}/rand/relic_rand_${RAND_PATH}.c")
	list(APPEND CORE_SRCS "rand/relic_rand_core.c")
	list(APPEND CORE_SRCS "rand/relic_rand_${RAND_PATH}.c")
endif(EXISTS "${CMAKE_CURRENT_SOURCE_DIR}/rand/relic_rand_${RAND_PATH}.c")

file(GLOB BN_SRCS bn/*.c)
file(GLOB DV_SRCS dv/*.c)
file(GLOB FP_SRCS fp/*.c)
file(GLOB FPX_SRCS fpx/*.c)
file(GLOB FB_SRCS fb/*.c)
file(GLOB FBX_SRCS fbx/*.c)
file(GLOB EP_SRCS ep/*.c)
file(GLOB EPX_SRCS epx/*.c)
file(GLOB EB_SRCS eb/*.c)
file(GLOB ED_SRCS ed/*.c)
file(GLOB PP_SRCS pp/*.c)
file(GLOB BC_SRCS bc/*.c)
file(GLOB MD_SRCS md/*.c)
file(GLOB EC_SRCS ec/*.c)
file(GLOB PC_SRCS pc/*.c)
file(GLOB MPC_SRCS mpc/*.c)

set(RELIC_SRCS ${CORE_SRCS})

if (TESTS GREATER 0)
	list(APPEND RELIC_SRCS "relic_test.c")
endif(TESTS GREATER 0)

if (BENCH GREATER 0)
	list(APPEND RELIC_SRCS "relic_bench.c")
endif(BENCH GREATER 0)

if (WITH_BN)
	list(APPEND RELIC_SRCS ${BN_SRCS})
	file(GLOB TEMP low/easy/relic_bn*.c)
	list(APPEND LOW_SRCS ${TEMP})
endif(WITH_BN)

if (WITH_DV)
	list(APPEND RELIC_SRCS ${DV_SRCS})
endif(WITH_DV)

if (WITH_FP)
	list(APPEND RELIC_SRCS ${FP_SRCS})
	file(GLOB TEMP low/easy/relic_fp_*.c)
	list(APPEND LOW_SRCS ${TEMP})
endif(WITH_FP)

if (WITH_FPX)
	list(APPEND RELIC_SRCS ${FPX_SRCS})
	file(GLOB TEMP low/easy/relic_fpx_*.c)
	list(APPEND LOW_SRCS ${TEMP})
endif(WITH_FPX)

if (WITH_FB)
	list(APPEND RELIC_SRCS ${FB_SRCS})
	file(GLOB TEMP low/easy/relic_fb*.c)
	list(APPEND LOW_SRCS ${TEMP})
endif(WITH_FB)

if (WITH_FBX)
	list(APPEND RELIC_SRCS ${FBX_SRCS})
endif(WITH_FBX)

if (WITH_EP)
	list(APPEND RELIC_SRCS ${EP_SRCS})
endif(WITH_EP)

if (WITH_EPX)
	list(APPEND RELIC_SRCS ${EPX_SRCS})
endif(WITH_EPX)

if (WITH_EB)
	list(APPEND RELIC_SRCS ${EB_SRCS})
endif(WITH_EB)

if (WITH_ED)
	list(APPEND RELIC_SRCS ${ED_SRCS})
endif(WITH_ED)

if (WITH_PP)
	list(APPEND RELIC_SRCS ${PP_SRCS})
endif(WITH_PP)

if (WITH_PC)
	list(APPEND RELIC_SRCS ${PC_SRCS})
endif(WITH_PC)

if (WITH_CP)
	if (WITH_BN)
		list(APPEND RELIC_SRCS "cp/relic_cp_rsa.c")
		list(APPEND RELIC_SRCS "cp/relic_cp_rabin.c")
		list(APPEND RELIC_SRCS "cp/relic_cp_bdpe.c")
		list(APPEND RELIC_SRCS "cp/relic_cp_ghpe.c")
		list(APPEND RELIC_SRCS "cp/relic_cp_phpe.c")
		list(APPEND RELIC_SRCS "cp/relic_cp_shpe.c")
		list(APPEND RELIC_SRCS "cp/relic_cp_rsapsi.c")
		list(APPEND RELIC_SRCS "cp/relic_cp_shipsi.c")
	endif()
	if (WITH_EB OR WITH_EP OR WITH_ED OR WITH_EC)
		list(APPEND RELIC_SRCS "cp/relic_cp_ecdh.c")
		list(APPEND RELIC_SRCS "cp/relic_cp_ecmqv.c")
		list(APPEND RELIC_SRCS "cp/relic_cp_ecies.c")
		list(APPEND RELIC_SRCS "cp/relic_cp_ecdsa.c")
		list(APPEND RELIC_SRCS "cp/relic_cp_ecss.c")
		list(APPEND RELIC_SRCS "cp/relic_cp_vbnn.c")
		list(APPEND RELIC_SRCS "cp/relic_cp_pok.c")
		list(APPEND RELIC_SRCS "cp/relic_cp_sok.c")
		list(APPEND RELIC_SRCS "cp/relic_cp_ers.c")
		list(APPEND RELIC_SRCS "cp/relic_cp_smlers.c")
		list(APPEND RELIC_SRCS "cp/relic_cp_etrs.c")
	endif()
	if (WITH_PP OR WITH_PC)
		list(APPEND RELIC_SRCS "cp/relic_cp_pcdel.c")
		list(APPEND RELIC_SRCS "cp/relic_cp_sokaka.c")
		list(APPEND RELIC_SRCS "cp/relic_cp_bgn.c")
		list(APPEND RELIC_SRCS "cp/relic_cp_ibe.c")
		list(APPEND RELIC_SRCS "cp/relic_cp_bls.c")
		list(APPEND RELIC_SRCS "cp/relic_cp_cls.c")
		list(APPEND RELIC_SRCS "cp/relic_cp_pss.c")
		list(APPEND RELIC_SRCS "cp/relic_cp_bbs.c")
		list(APPEND RELIC_SRCS "cp/relic_cp_zss.c")
		list(APPEND RELIC_SRCS "cp/relic_cp_cmlhs.c")
		list(APPEND RELIC_SRCS "cp/relic_cp_mklhs.c")
		list(APPEND RELIC_SRCS "cp/relic_cp_pbpsi.c")
	endif()
	if (WITH_MPC)
		list(APPEND RELIC_SRCS "cp/relic_cp_mpss.c")
	endif()
endif()

if (WITH_BC)
	list(APPEND RELIC_SRCS ${BC_SRCS})
endif(WITH_BC)

if (WITH_MD)
	list(APPEND RELIC_SRCS ${MD_SRCS})
endif(WITH_MD)

if (WITH_MPC)
	list(APPEND RELIC_SRCS ${MPC_SRCS})
endif(WITH_MPC)

foreach(SRC ${LOW_SRCS})
	get_filename_component(SRC ${SRC} NAME_WE)
	set(FILE "${CMAKE_CURRENT_SOURCE_DIR}/low/${ARITH_PATH}/${SRC}")
	if (EXISTS "${FILE}.c")
		list(APPEND ARITH_SRCS "${FILE}.c")
	endif(EXISTS "${FILE}.c")
	if (EXISTS "${FILE}.s")
		list(APPEND ARITH_ASMS "${FILE}.s")
	endif(EXISTS "${FILE}.s")
	if (NOT EXISTS "${FILE}.c" AND NOT EXISTS "${FILE}.s")
		if (EXISTS "${CMAKE_CURRENT_SOURCE_DIR}/low/${INHERIT_PATH}/${SRC}.c")
			list(APPEND ARITH_SRCS "low/${INHERIT_PATH}/${SRC}.c")
		else(EXISTS "${CMAKE_CURRENT_SOURCE_DIR}/low/${INHERIT_PATH}/${SRC}.c")
			list(APPEND ARITH_SRCS "low/easy/${SRC}.c")
		endif(EXISTS "${CMAKE_CURRENT_SOURCE_DIR}/low/${INHERIT_PATH}/${SRC}.c")
	endif(NOT EXISTS "${FILE}.c" AND NOT EXISTS "${FILE}.s")
endforeach(SRC)

list(APPEND RELIC_SRCS ${ARITH_SRCS})

list(APPEND RELIC_SRCS ${ARITH_ASMS})
set_source_files_properties(${ARITH_ASMS} PROPERTIES LANGUAGE C COMPILE_FLAGS "-DASM -x assembler-with-cpp")

add_custom_target(arith_objs DEPENDS ${ARITH_OBJS})

macro(LINK_LIBS LIBRARY)
	if(OPSYS STREQUAL LINUX)
		target_link_libraries(${LIBRARY} rt)
	endif(OPSYS STREQUAL LINUX)
	if(MULTI STREQUAL OPENMP AND NOT MSVC)
		target_link_libraries(${LIBRARY} gomp m)
	endif(MULTI STREQUAL OPENMP AND NOT MSVC)
	if(MULTI STREQUAL PTHREAD)
		target_link_libraries(${LIBRARY} pthread m)
	endif(MULTI STREQUAL PTHREAD)
endmacro(LINK_LIBS)

function(AMALGAMATE output inputFiles)
	file(WRITE "${output}" "// This file is an automatic amalgamation of all relic source files.\n" )
	FOREACH(file ${inputFiles})
		file(APPEND "${output}" "#include \"${file}\"\n")
	ENDFOREACH()
endfunction(AMALGAMATE)

if(AMALG)
	AMALGAMATE("relic-all.c" "${RELIC_SRCS}${ARITH_OBJS}")
endif(AMALG)

if (SHLIB)
	if(AMALG)
		add_library(${RELIC} SHARED "relic-all.c")
	else(AMALG)
		add_library(${RELIC} SHARED ${RELIC_SRCS} ${ARITH_OBJS})
	endif(AMALG)
	add_dependencies(${RELIC} arith_objs)
	target_link_libraries(${RELIC} ${ARITH_LIBS})
	link_libs(${RELIC})
	install(TARGETS ${RELIC} LIBRARY DESTINATION lib ARCHIVE DESTINATION lib)
endif(SHLIB)

if (STLIB)
	if(AMALG)
		add_library(${RELIC_S} STATIC "relic-all.c")
	else(AMALG)
		add_library(${RELIC_S} STATIC ${RELIC_SRCS} ${ARITH_OBJS})
	endif(AMALG)
	add_dependencies(${RELIC_S} arith_objs)
	target_link_libraries(${RELIC_S} ${ARITH_LIBS})
	link_libs(${RELIC_S})
	install(TARGETS ${RELIC_S} ARCHIVE DESTINATION lib)
endif(STLIB)
