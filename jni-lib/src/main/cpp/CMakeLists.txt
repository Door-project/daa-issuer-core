#add_library(tools SHARED "")
#target_sources(tools PRIVATE tools.cpp)

#add_library(daabridgecpp SHARED "")
#target_sources(daabridgecpp PRIVATE native-lib.cpp)


message(STATUS "project source dir: ${PROJECT_SOURCE_DIR}")



project("daabridgecpp")

set (COMPILE_FLAGS "-O2")
add_definitions(-DTPM_POSIX -DTPM_TPM20)

find_package(OpenSSL REQUIRED)

link_directories(/home/menelaos/libs/ibmtss/utils)
include_directories(/home/menelaos/libs/ibmtss/utils)

link_directories(/home/menelaos/libs/cJSON/build)
include_directories(/home/menelaos/libs/cJSON)

link_directories(/home/menelaos/libs/incubator-milagro-crypto-c/lib)
include_directories(/home/menelaos/libs/incubator-milagro-crypto-c/include)

add_library( # Sets the name of the library.
        daabridgecpp

        # Sets the library as a shared library.
        SHARED
        ""
        )

target_sources(
        daabridgecpp
        PRIVATE
        daa/defines.h daa/daa_client_base.h daa/daa_client_base.c daa/daa_issuer.h daa/BN_Crypto.h daa/BN_Crypto.c daa/daa_issuer.c daa/issPk.h daa/issPk.c daa/daa_pairings.h daa/daa_pairings.c
        daa_bridge.h daa_bridge.c persistence.h defines.h bridge_tpm.h bridge_tpm.c persistence.c hash.h hash.c policy.h cryptoutils.c cryptoutils.h  templates.h templates.c policy.c objecttemplates.h objecttemplates.c Test_issuer/issuer_interface.h Test_issuer/issuer_interface.c
        native-lib.cpp
)

target_link_libraries(daabridgecpp m crypto cjson ibmtss amcl_bls_FP256BN amcl_mpin_FP256BN amcl_curve_FP256BN amcl_pairing_FP256BN amcl_wcc_FP256BN)


set(RESOURCES_PATH "${PROJECT_SOURCE_DIR}/../../../src/main/resources")
file(COPY "/home/menelaos/libs/cJSON/build/" DESTINATION "${RESOURCES_PATH}")
file(COPY "/home/menelaos/libs/ibmtss/utils/" DESTINATION "${RESOURCES_PATH}")
file(COPY "/home/menelaos/libs/incubator-milagro-crypto-c/lib/" DESTINATION "${RESOURCES_PATH}")