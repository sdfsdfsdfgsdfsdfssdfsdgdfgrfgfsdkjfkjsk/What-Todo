cmake_minimum_required(VERSION 3.14)

project(todo VERSION 0.1 LANGUAGES CXX)

set(CMAKE_AUTOUIC ON)
set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTORCC ON)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Find Qt package
find_package(Qt6 REQUIRED COMPONENTS Core Quick)
# For compatibility with Qt5, uncomment the following line
# find_package(Qt5 REQUIRED COMPONENTS Core Quick)

set(PROJECT_SOURCES
    main.cpp
    qml.qrc
)

if(Qt6_FOUND)
    qt_add_executable(todo
        MANUAL_FINALIZATION
        ${PROJECT_SOURCES}
    )
else()
    add_executable(todo
        ${PROJECT_SOURCES}
    )
endif()

target_link_libraries(todo
    PRIVATE Qt6::Core Qt6::Quick
    # For Qt5, use the following line instead
    # PRIVATE Qt5::Core Qt5::Quick
)

# Set target properties
set_target_properties(todo PROPERTIES
    MACOSX_BUNDLE TRUE
    WIN32_EXECUTABLE TRUE
)

include(GNUInstallDirs)
install(TARGETS todo
    RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
)

if(Qt6_FOUND)
    qt_import_qml_plugins(todo)
    qt_finalize_executable(todo)
endif()
