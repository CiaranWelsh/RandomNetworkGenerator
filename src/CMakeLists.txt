# configure config file into build tree
configure_file(
        ${CMAKE_CURRENT_SOURCE_DIR}/RandomNetworkGeneratorConfig.h.in
        ${CMAKE_CURRENT_BINARY_DIR}/RandomNetworkGeneratorConfig.h
)















# Write mytools_export.h to the current binary directory
#include(GenerateExportHeader)
#generate_export_header()





