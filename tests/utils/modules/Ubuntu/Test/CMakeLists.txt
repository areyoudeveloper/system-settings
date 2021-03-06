add_definitions(-DQT_NO_KEYWORDS)

set(UbuntuTestQML_SOURCES
    testutil.cpp
    plugin.cpp
    TouchEventSequenceWrapper.cpp
)

include_directories(
    ${CMAKE_CURRENT_SOURCE_DIR}
    ${Qt5Gui_PRIVATE_INCLUDE_DIRS}
    )

add_library(UbuntuTestQml MODULE ${UbuntuTestQML_SOURCES})

target_link_libraries(UbuntuTestQml Qt5::Core Qt5::Quick Qt5::Test)

# copy qml files into build directory for shadow builds
file(GLOB QML_JS_FILES *.js *.qml qmldir)

add_custom_target(UbuntuTestQmlJsFiles ALL
    COMMAND cp ${QML_JS_FILES} ${CMAKE_CURRENT_BINARY_DIR}
    DEPENDS ${QML_JS_FILES}
)

add_custom_target(UbuntuTest)
add_dependencies(UbuntuTest UbuntuTestQml UbuntuTestQmlJsFiles)
