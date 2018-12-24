cmake_minimum_required( VERSION 3.5 )

if(LLVM_ROOT)
    if ("${LLVM_INCLUDE}" STREQUAL "")
        set(LLVM_INCLUDE "${LLVM_ROOT}/include")
    endif()
    if ("${LLVM_LIB}" STREQUAL "")
        set(LLVM_LIB "${LLVM_ROOT}/lib")
    endif()
endif(LLVM_ROOT)

if(CLANG_ROOT)
    if ("${CLANG_INCLUDE}" STREQUAL "")
        set(CLANG_INCLUDE "${CLANG_ROOT}/include")
    endif()
    if ("${CLANG_LIB}" STREQUAL "")
        set(CLANG_LIB "${CLANG_ROOT}/lib")
    endif()
endif(CLANG_ROOT)

if(BOOST_ROOT)
    if ("${BOOST_INCLUDE}" STREQUAL "")
        set(BOOST_INCLUDE "${BOOST_ROOT}/include")
    endif()
    if ("${BOOST_LIB}" STREQUAL "")
        set(BOOST_LIB "${BOOST_ROOT}/lib")
    endif()
endif(BOOST_ROOT)

message("CLANGROOT:${CLANG_ROOT}")

if (WIN32)
	set(SYSTEM_LIB version)
elseif (APPLE)
	set(SYSTEM_LIB z dl pthread m curses)
else()
	set(SYSTEM_LIB z rt dl tinfo pthread m)
endif()

if(NOT EXISTS "${PROJECT_SOURCE_DIR}/deps/rapidjson")
    include(ProjectRapidJson)
endif()

set(RAPIDJSON_INCLUDE "${PROJECT_SOURCE_DIR}/deps/rapidjson/include")

if("${CMAKE_INSTALL_FULL_BINDIR}" STREQUAL "")
    set(CMAKE_INSTALL_FULL_BINDIR "${PROJECT_SOURCE_DIR}/bin")
endif()


set( CMAKE_CXX_STANDARD 11 )

set (CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -g -DRAPIDJSON_HAS_STDSTRING")

set(SOURCES main.cpp AbiGenerator.cpp AbiMacro.cpp AbiJson.cpp Log.cpp StringUtil.cpp Template.cpp)

add_executable(platon-abigen ${SOURCES})

target_include_directories(platon-abigen PUBLIC ${CLANG_INCLUDE} ${LLVM_INCLUDE} ${RAPIDJSON_INCLUDE} ${BOOST_INCLUDE})

target_link_libraries(platon-abigen
	${CLANG_LIB}/libclangBasic.a
    ${CLANG_LIB}/libclangFormat.a
    ${CLANG_LIB}/libclangRewrite.a
    ${CLANG_LIB}/libclangFrontendTool.a
    ${CLANG_LIB}/libclangTooling.a
    ${CLANG_LIB}/libclangFrontend.a
    ${CLANG_LIB}/libclangToolingCore.a
    ${CLANG_LIB}/libclangDriver.a
    ${CLANG_LIB}/libclangSerialization.a
    ${CLANG_LIB}/libclangParse.a
    ${CLANG_LIB}/libclangSema.a
    ${CLANG_LIB}/libclangAnalysis.a
    ${CLANG_LIB}/libclangAST.a
    ${CLANG_LIB}/libclangBasic.a
    ${CLANG_LIB}/libclangEdit.a
    ${CLANG_LIB}/libclangLex.a

	${LLVM_LIB}/libLLVMExecutionEngine.a
	${LLVM_LIB}/libLLVMRuntimeDyld.a
	${LLVM_LIB}/libLLVMDebugInfoDWARF.a
	${LLVM_LIB}/libLLVMX86Disassembler.a
	${LLVM_LIB}/libLLVMX86AsmParser.a
	${LLVM_LIB}/libLLVMX86CodeGen.a
	${LLVM_LIB}/libLLVMAsmPrinter.a
	${LLVM_LIB}/libLLVMDebugInfoCodeView.a
	${LLVM_LIB}/libLLVMDebugInfoMSF.a
	${LLVM_LIB}/libLLVMGlobalISel.a
	${LLVM_LIB}/libLLVMSelectionDAG.a
	${LLVM_LIB}/libLLVMX86Desc.a
	${LLVM_LIB}/libLLVMX86AsmPrinter.a
	${LLVM_LIB}/libLLVMX86Info.a
	${LLVM_LIB}/libLLVMX86Utils.a
	${LLVM_LIB}/libLLVMCodeGen.a
	${LLVM_LIB}/libLLVMipo.a
	${LLVM_LIB}/libLLVMBitWriter.a
	${LLVM_LIB}/libLLVMIRReader.a
	${LLVM_LIB}/libLLVMAsmParser.a
	${LLVM_LIB}/libLLVMInstrumentation.a
	${LLVM_LIB}/libLLVMLinker.a
	${LLVM_LIB}/libLLVMVectorize.a
	${LLVM_LIB}/libLLVMScalarOpts.a
	${LLVM_LIB}/libLLVMInstCombine.a
	${LLVM_LIB}/libLLVMTransformUtils.a
	${LLVM_LIB}/libLLVMTarget.a
	${LLVM_LIB}/libLLVMAnalysis.a
	${LLVM_LIB}/libLLVMOption.a
	${LLVM_LIB}/libLLVMMCDisassembler.a
	${LLVM_LIB}/libLLVMProfileData.a
	${LLVM_LIB}/libLLVMObject.a
	${LLVM_LIB}/libLLVMMCParser.a
	${LLVM_LIB}/libLLVMMC.a
	${LLVM_LIB}/libLLVMBitReader.a
	${LLVM_LIB}/libLLVMCore.a
	${LLVM_LIB}/libLLVMSupport.a
	${LLVM_LIB}/libLLVMDemangle.a
    
    ${BOOST_LIB}/libboost_filesystem.a
    ${BOOST_LIB}/libboost_system.a
    ${BOOST_LIB}/libboost_log.a
    ${BOOST_LIB}/libboost_log_setup.a
    ${BOOST_LIB}/libboost_thread.a
    ${BOOST_LIB}/libboost_random.a

    ${SYSTEM_LIB}
    )

install( TARGETS platon-abigen
     RUNTIME DESTINATION ${CMAKE_INSTALL_FULL_BINDIR}
    )
