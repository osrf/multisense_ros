#
# SaveImageUtility - Makefile
#

#
# Include all of our child directories.
#

include_directories (
        ${BASE_DIRECTORY}${SOURCE_DIRECTORY}/source
                    )
#
# Setup the executable that we will use.
#

add_executable(SaveImageUtility SaveImageUtility.cc)

#
# Specify libraries against which to link.
#

target_link_libraries(SaveImageUtility MultiSense
                                       pthread
                                       rt
                                       png)
