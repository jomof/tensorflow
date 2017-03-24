cmake_minimum_required(VERSION 3.7)
project(tensorflow_inference)

include(.cdep/modules/cdep-dependencies-config.cmake REQUIRED)

set(tensorflow_root "../..")
set(download_root ../makefile/downloads)
set(gen_root ../makefile/gen)

file(GLOB_RECURSE tensorflow_cc
    ${tensorflow_root}/core/lib/strings/*.cc
    ${tensorflow_root}/core/platform/default/*.cc
    )


file(GLOB_RECURSE tensorflow_remove
    ${tensorflow_root}/core/*_test*.cc
    ${tensorflow_root}/core/*cuda_libdevice_path.cc
    )
list(REMOVE_ITEM tensorflow_cc ${tensorflow_remove})

add_library(tensorflow_inference STATIC ${tensorflow_cc})
target_include_directories(tensorflow_inference PRIVATE "../../..")
target_include_directories(tensorflow_inference PRIVATE ${download_root}/eigen)
target_include_directories(tensorflow_inference PRIVATE ${gen_root}/proto)
#target_include_directories(tensorflow_inference PRIVATE ${gen_root}/proto_text)

add_all_cdep_dependencies(tensorflow_inference)


file(GLOB tensorflow_core_platform_h ${tensorflow_root}/core/platform/*.h)
file(GLOB tensorflow_core_platform_default_h ${tensorflow_root}/core/platform/default/*.h)

file(COPY ${tensorflow_root}/core/lib/strings/stringprintf.h DESTINATION ${CMAKEIFY_REDIST_INCLUDE_DIRECTORY}/tensorflow/core/lib/strings )
file(COPY ${tensorflow_core_platform_h} DESTINATION ${CMAKEIFY_REDIST_INCLUDE_DIRECTORY}/tensorflow/core/platform )
file(COPY ${tensorflow_core_platform_default_h} DESTINATION ${CMAKEIFY_REDIST_INCLUDE_DIRECTORY}/tensorflow/core/platform/default )

