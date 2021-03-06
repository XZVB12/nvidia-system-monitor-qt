# 3.10, but tested only under 3.15+
# should work with previous versions too,
# if you use one, just edit next line
cmake_minimum_required(VERSION 3.10)
project(qnvsm)

set(CMAKE_AUTOMOC ON)
set(CMAKE_CXX_STANDARD 17)

set(Version 1.2)

if (NOT VersionPrefix)
    set(VersionPrefix "(compiled from source)")
endif()

find_package(Qt5Core)
find_package(Qt5Widgets)

if (NOT IconPath)
    set(IconPath "")
endif()

add_definitions(-DICON_PATH="${IconPath}")

set(QNVSM_Version "${Version} ${VersionPrefix}")
configure_file(src/resources/MainWindow_about.html src/resources/MainWindow_about.html.out)

file(READ ${CMAKE_CURRENT_BINARY_DIR}/src/resources/MainWindow_about.html.out Resources_MainWindow_About)
file(READ src/resources/MainWindow_help.html Resources_MainWindow_Help)
configure_file(src/resources/MainWindowResources.h.in src/resources/MainWindowResources.h)

file(READ src/resources/QSplitter.qss Resources_QSplitter_Stylesheet)
configure_file(src/resources/QSplitterResources.h.in src/resources/QSplitterResources.h)

include_directories(src)
include_directories(${CMAKE_CURRENT_BINARY_DIR}/src)

add_executable(qnvsm
        src/main.cpp
        src/MainWindow.cpp src/MainWindow.h
        src/SettingsDialog.cpp src/SettingsDialog.h
        src/SelectColorButton.cpp src/SelectColorButton.h
        src/resources/MainWindowResources.h.in src/resources/MainWindowResources.h
        src/resources/QSplitterResources.h.in src/resources/QSplitterResources.h
        src/core/Utils.cpp src/core/Utils.h
        src/core/Worker.cpp src/core/Worker.h
        src/core/WorkerThread.cpp src/core/WorkerThread.h
        src/core/NVSMIParser.cpp src/core/NVSMIParser.h
        src/core/SettingsManager.cpp src/core/SettingsManager.h
        src/processes/ProcessInfo.cpp src/processes/ProcessInfo.h
        src/processes/ProcessesWorker.cpp src/processes/ProcessesWorker.h
        src/processes/ProcessesView.cpp src/processes/ProcessesView.h
        src/utilization/Point.h src/utilization/Point.cpp
        src/utilization/CircleWidget.cpp src/utilization/CircleWidget.h
        src/utilization/UtilizationData.h src/utilization/memory/MemoryData.h
        src/utilization/UtilizationWorker.cpp src/utilization/UtilizationWorker.h
        src/utilization/UtilizationWidget.cpp src/utilization/UtilizationWidget.h
        src/utilization/UtilizationContainer.cpp src/utilization/UtilizationContainer.h
        src/utilization/UtilizationContainerCommon.h
        src/utilization/gpu/GPUUtilizationWorker.cpp src/utilization/gpu/GPUUtilizationWorker.h
        src/utilization/gpu/GPUUtilizationWidget.cpp src/utilization/gpu/GPUUtilizationWidget.h
        src/utilization/memory/MemoryUtilizationWorker.cpp src/utilization/memory/MemoryUtilizationWorker.h
        src/utilization/memory/MemoryUtilizationWidget.cpp src/utilization/memory/MemoryUtilizationWidget.h
        src/utilization/gpu/GPUUtilizationContainer.cpp src/utilization/gpu/GPUUtilizationContainer.h
        src/utilization/memory/MemoryUtilizationContainer.cpp src/utilization/memory/MemoryUtilizationContainer.h)

target_link_libraries(qnvsm ${Qt5Core_LIBRARIES} ${Qt5Widgets_LIBRARIES})
