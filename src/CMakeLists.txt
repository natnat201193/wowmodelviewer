cmake_minimum_required(VERSION 2.6)
project(WoWModelViewer)
include(${CMAKE_CURRENT_LIST_DIR}/cmake/common.cmake)

# group cmake auto targets  in a dedicated folder for Visual Studio
set_property(GLOBAL PROPERTY USE_FOLDERS ON)
set_property(GLOBAL PROPERTY AUTOGEN_TARGETS_FOLDER QTAutoMoc)

if(DEFINED BUILDNUMBER)
  add_definitions(-D_BUILDNUMBER=${BUILDNUMBER})
endif()

# add external libraries compilation
add_subdirectory(3rdparty/CxImage)

# configure CASC lib for our usage
set(CASCLIB_INSTALL_LOCATION ${CMAKE_BINARY_DIR}/casclib)
add_subdirectory(3rdparty/casclib)

# add internal libraries compilation 
add_subdirectory(core)
add_subdirectory(games)

# then trigs executables compilation
add_subdirectory(wowmodelviewer)
add_subdirectory(UpdateManager)

# add plugins compilation
add_subdirectory(plugins)

add_custom_target(release
          COMMAND
          "${CMAKE_COMMAND}" --build . --target clean --config Release
          COMMAND
          "${CMAKE_COMMAND}" --build . --target install --config Release
                  COMMAND
          $ENV{WMV_SDK_BASEDIR}/NSIS/makensis.exe "${WMV_BASE_PATH}/Installers/Windows/NSIS/WMVInstallerMUI.nsi"
          )
