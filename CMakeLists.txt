cmake_minimum_required(VERSION 3.5)

if(NOT DEFINED ENV{VITASDK})
    message(FATAL_ERROR "Please define VITASDK to point to your vita SDK path!")
endif()

set(CMAKE_TOOLCHAIN_FILE "$ENV{VITASDK}/share/vita.toolchain.cmake" CACHE PATH "toolchain file")

project(adrenaline)
include("${VITASDK}/share/vita.cmake" REQUIRED)

add_subdirectory(kernel)
add_subdirectory(user)
add_subdirectory(vsh)
add_subdirectory(usbdevice)
add_subdirectory(bubble)


link_directories(${CMAKE_CURRENT_BINARY_DIR}/kernel)
link_directories(${CMAKE_BINARY_DIR}/kernel)

add_dependencies(${PROJECT_NAME} "kernel_all")
add_dependencies(${PROJECT_NAME} "user_all")
add_dependencies(${PROJECT_NAME} "vsh_all")
add_dependencies(${PROJECT_NAME} "usbdevice_all")
add_dependencies(adrenaline_user "kernel_all")

add_custom_target(updater
  COMMAND bin2c ${CMAKE_CURRENT_BINARY_DIR}/user/adrenaline_user.suprx ${CMAKE_SOURCE_DIR}/cef/updater/adrenaline_user.h adrenaline_user
  COMMAND bin2c ${CMAKE_CURRENT_BINARY_DIR}/kernel/adrenaline_kernel.skprx ${CMAKE_SOURCE_DIR}/cef/updater/adrenaline_kernel.h adrenaline_kernel
  COMMAND bin2c ${CMAKE_CURRENT_BINARY_DIR}/vsh/adrenaline_vsh.suprx ${CMAKE_SOURCE_DIR}/cef/updater/adrenaline_vsh.h adrenaline_vsh
  COMMAND bin2c ${CMAKE_SOURCE_DIR}/bubble/pkg/frame.xml ${CMAKE_SOURCE_DIR}/cef/updater/frame_xml.h frame_xml
  COMMAND bin2c ${CMAKE_SOURCE_DIR}/bubble/pkg/sce_sys/livearea/contents/template.xml ${CMAKE_SOURCE_DIR}/cef/updater/template_xml.h template_xml
  COMMAND make -C ${CMAKE_SOURCE_DIR}/cef/updater
  COMMAND mkdir -p ${CMAKE_CURRENT_BINARY_DIR}/updater
  COMMAND cp ${CMAKE_SOURCE_DIR}/cef/updater/EBOOT.PBP ${CMAKE_CURRENT_BINARY_DIR}/updater
  COMMAND cp ${CMAKE_SOURCE_DIR}/cef/updater/psp-updatelist.txt ${CMAKE_CURRENT_BINARY_DIR}/updater
  DEPENDS user_all kernel_all vsh_all
)

#add_dependencies(updater "kernel_all")
#add_dependencies(updater "user_all")
#add_dependencies(updater "vsh_all")
