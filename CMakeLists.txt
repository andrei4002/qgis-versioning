cmake_minimum_required( VERSION 2.8 )
project( versioning )

file(GLOB Files 
    ${CMAKE_CURRENT_SOURCE_DIR}/metadata.txt
    ${CMAKE_CURRENT_SOURCE_DIR}/*.py 
    ${CMAKE_CURRENT_SOURCE_DIR}/*.qrc 
    ${CMAKE_CURRENT_SOURCE_DIR}/*.svg 
    )

foreach(File ${Files})
    get_filename_component(BaseName ${File} NAME)
    configure_file(${File} ${CMAKE_BINARY_DIR}/${BaseName} COPYONLY)
endforeach()

file(GLOB SVG *.svg)
add_custom_command( OUTPUT resources_rc.py 
    COMMAND pyrcc4 -o resources_rc.py resources.qrc 
    WORKING_DIRECTORY ${CMAKE_BINARY_DIR}
    DEPENDS ${CMAKE_BINARY_DIR}/resources.qrc ${SVG} )

add_custom_target(qgis_plugin ALL DEPENDS resources_rc.py ${Files})