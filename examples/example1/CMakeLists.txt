project (ngrestdb_example1 CXX)

set(NGRESTDB_EXAMPLE1_HEADERS datatypes.h)

set(PROJECT_SOURCE_DIR ${CMAKE_CURRENT_SOURCE_DIR}/src)
set(NGRESTDB_EXAMPLE1_CODEGEN_DIR "${PROJECT_BINARY_DIR}/codegen")

PREPEND(NGRESTDB_EXAMPLE1_HEADERS_PATHS ${PROJECT_SOURCE_DIR} ${NGRESTDB_EXAMPLE1_HEADERS})

CODEGEN_FILES(NGRESTDB_EXAMPLE1_CODEGEN_SOURCES ${NGRESTDB_EXAMPLE1_CODEGEN_DIR} ${NGRESTDB_EXAMPLE1_HEADERS})

add_custom_command(OUTPUT ${NGRESTDB_EXAMPLE1_CODEGEN_SOURCES}
    COMMAND ${NGRESTCG_BIN} -e -i "${PROJECT_SOURCE_DIR}" -o ${NGRESTDB_EXAMPLE1_CODEGEN_DIR} -t dbentity ${NGRESTDB_EXAMPLE1_HEADERS} -x
    DEPENDS ${NGRESTDB_EXAMPLE1_HEADERS_PATHS}
)

file(GLOB NGRESTDB_EXAMPLE1_SOURCES ${PROJECT_SOURCE_DIR}/*.cpp)

list(APPEND NGRESTDB_EXAMPLE1_SOURCES ${NGRESTDB_EXAMPLE1_CODEGEN_SOURCES})

include_directories(${PROJECT_SOURCE_DIR} ${NGRESTDB_EXAMPLE1_CODEGEN_DIR})

add_executable(ngrestdb_example1 ${NGRESTDB_EXAMPLE1_SOURCES})

set_target_properties(ngrestdb_example1 PROPERTIES PREFIX "")
set_target_properties(ngrestdb_example1 PROPERTIES
    LIBRARY_OUTPUT_DIRECTORY "${PROJECT_SERVICES_DIR}"
)

target_link_libraries(ngrestdb_example1 ngrestutils ngrestdbcommon)

if (HAS_SQLITE)
    target_link_libraries(ngrestdb_example1 ngrestdbsqlite)
endif()
if (HAS_MYSQL)
    target_link_libraries(ngrestdb_example1 ngrestdbmysql)
endif()
if (HAS_POSTGRES)
    target_link_libraries(ngrestdb_example1 ngrestdbpostgres)
endif()
