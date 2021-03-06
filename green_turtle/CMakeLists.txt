INCLUDE_DIRECTORIES(${CMAKE_SOURCE_DIR})

FILE(GLOB GREEN_TURTLE          *.h *.cc)
FILE(GLOB GREEN_TURTLE_NET      net/*.h net/*.cc)

LIST(REMOVE_ITEM GREEN_TURTLE_NET
    ${CMAKE_CURRENT_SOURCE_DIR}/net/epoll_poller.h
    ${CMAKE_CURRENT_SOURCE_DIR}/net/epoll_poller.cc
    ${CMAKE_CURRENT_SOURCE_DIR}/net/kqueue_poller.h
    ${CMAKE_CURRENT_SOURCE_DIR}/net/kqueue_poller.cc
    )

IF(${CMAKE_SYSTEM_NAME} MATCHES "Linux")
    MESSAGE(STATUS "Linux : Add Epoll Support")
    SET(GREEN_TURTLE_NET ${GREEN_TURTLE_NET} net/epoll_poller.h net/epoll_poller.cc)
ENDIF(${CMAKE_SYSTEM_NAME} MATCHES "Linux")

IF((${CMAKE_SYSTEM_NAME} MATCHES "BSD") OR (${CMAKE_SYSTEM_NAME} MATCHES "Darwin"))
    MESSAGE(STATUS "BSD Like : Add Kqueue Support")
    SET(GREEN_TURTLE_NET ${GREEN_TURTLE_NET} net/kqueue_poller.h net/kqueue_poller.cc)
ENDIF((${CMAKE_SYSTEM_NAME} MATCHES "BSD") OR (${CMAKE_SYSTEM_NAME} MATCHES "Darwin"))

SOURCE_GROUP(""         FILES ${GREEN_TURTLE})
SOURCE_GROUP("net"      FILES ${GREEN_TURTLE_NET})


ADD_LIBRARY(green_turtle STATIC
    ${GREEN_TURTLE}
    ${GREEN_TURTLE_NET}
    )
TARGET_LINK_LIBRARIES(green_turtle pthread)
