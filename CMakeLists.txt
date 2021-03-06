##
#  CMake script for the PRISMS-PF applications:
##

CMAKE_MINIMUM_REQUIRED(VERSION 2.8.8)

# Find deal.II installation
FIND_PACKAGE(deal.II 8.3.0 REQUIRED
	HINTS ${DEAL_II_DIR} ../ ../../ $ENV{DEAL_II_DIR})

DEAL_II_INITIALIZE_CACHED_VARIABLES()

# Set up the debug, release, and run targets
ADD_CUSTOM_TARGET(debug
  COMMAND +env ${CMAKE_COMMAND} -DCMAKE_BUILD_TYPE=Debug ${CMAKE_SOURCE_DIR}
  COMMAND +env ${CMAKE_COMMAND} --build ${CMAKE_BINARY_DIR} --target all
  COMMENT "Switch CMAKE_BUILD_TYPE to Debug"
  )

ADD_CUSTOM_TARGET(release
  COMMAND +env ${CMAKE_COMMAND} -DCMAKE_BUILD_TYPE=Release ${CMAKE_SOURCE_DIR}
  COMMAND +env ${CMAKE_COMMAND} --build ${CMAKE_BINARY_DIR} --target all
  COMMENT "Switch CMAKE_BUILD_TYPE to Release"
  )

ADD_CUSTOM_TARGET(run COMMAND main
  COMMENT "Run with ${CMAKE_BUILD_TYPE} configuration"
  )


# Set up the paths to the library files
INCLUDE_DIRECTORIES(include)

FILE(GLOB userInputParameters src/userInputParameters/*.cc)
FILE(GLOB EBVPfunctions src/ellipticBVP/*.cc)
FILE(GLOB CP_masterclass src/materialModels/crystalPlasticity/*.cc)
FILE(GLOB crystalOrientationsIO src/utilityObjects/*.cc)

PROJECT(prisms_cp)
SET(CMAKE_BUILD_TYPE Release)

# Append extra flags for the GNU compiler to suppress some warnings
if ("${CMAKE_CXX_COMPILER_ID}" STREQUAL "GNU")
        #set(DEAL_II_CXX_FLAGS_DEBUG "${DEAL_II_CXX_FLAGS_DEBUG} -Wno-maybe-uninitialized -Wno-unused-parameter -Wno-extra")
        #set(DEAL_II_CXX_FLAGS_RELEASE "${DEAL_II_CXX_FLAGS_RELEASE} -Wno-maybe-uninitialized -Wno-unused-parameter -Wno-extra")
        set(DEAL_II_CXX_FLAGS_DEBUG "${DEAL_II_CXX_FLAGS_DEBUG} -Wno-maybe-uninitialized -Wno-deprecated-declarations -Wno-comment -Wno-unused-parameter -Wno-unused-variable -Wno-unused-but-set-variable")
        set(DEAL_II_CXX_FLAGS_RELEASE "${DEAL_II_CXX_FLAGS_RELEASE} -Wno-maybe-uninitialized -Wno-deprecated-declarations -Wno-comment -Wno-unused-parameter -Wno-unused-variable -Wno-unused-but-set-variable")
        #set(CMAKE_CXX_FLAGS "-Wno-maybe-uninitialized -Wno-deprecated-declarations -Wno-comment -Wno-unused-parameter -Wno-unused-variable -Wno-unused-but-set-variable")
endif()

ADD_LIBRARY(${PROJECT_NAME} ${userInputParameters} ${EBVPfunctions} ${CP_masterclass} ${crystalOrientationsIO})
DEAL_II_SETUP_TARGET(${PROJECT_NAME})

PROJECT(prisms_cp_debug)
SET(CMAKE_BUILD_TYPE Debug)

# Append extra flags for the GNU compiler to suppress some warnings
if ("${CMAKE_CXX_COMPILER_ID}" STREQUAL "GNU")
        #set(DEAL_II_CXX_FLAGS_DEBUG "${DEAL_II_CXX_FLAGS_DEBUG} -Wno-maybe-uninitialized -Wno-unused-parameter -Wno-extra")
        #set(DEAL_II_CXX_FLAGS_RELEASE "${DEAL_II_CXX_FLAGS_RELEASE} -Wno-maybe-uninitialized -Wno-unused-parameter -Wno-extra")
        set(DEAL_II_CXX_FLAGS_DEBUG "${DEAL_II_CXX_FLAGS_DEBUG} -Wno-maybe-uninitialized -Wno-deprecated-declarations -Wno-comment -Wno-unused-parameter -Wno-unused-variable -Wno-unused-but-set-variable")
        set(DEAL_II_CXX_FLAGS_RELEASE "${DEAL_II_CXX_FLAGS_RELEASE} -Wno-maybe-uninitialized -Wno-deprecated-declarations -Wno-comment -Wno-unused-parameter -Wno-unused-variable -Wno-unused-but-set-variable")
        #set(CMAKE_CXX_FLAGS "-Wno-maybe-uninitialized -Wno-deprecated-declarations -Wno-comment -Wno-unused-parameter -Wno-unused-variable -Wno-unused-but-set-variable")
endif()

ADD_LIBRARY(${PROJECT_NAME} ${userInputParameters} ${EBVPfunctions} ${CP_masterclass} ${crystalOrientationsIO})
DEAL_II_SETUP_TARGET(${PROJECT_NAME})
