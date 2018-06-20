MACRO(add_example name)
    ADD_EXECUTABLE(${name} ${name}.cpp)
    TARGET_LINK_LIBRARIES(${name} SinsyRemix)
ENDMACRO()

add_example(sinsy)

install(TARGETS  sinsy DESTINATION build)
