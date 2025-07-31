
if(NOT CONFIG_OPUS)
  return()
endif()

get_filename_component(OPUS_DIR "${ZEPHYR_CURRENT_MODULE_DIR}" ABSOLUTE)

zephyr_library()

zephyr_library_compile_definitions(
  USE_ALLOCA
  HAVE_ALLOCA_H
  HAVE_LRINT
  HAVE_LRINTF
  FIXED_POINT
  DISABLE_FLOAT_API
  HAVE_MEMORY_H
  OPUS_BUILD
)
zephyr_library_compile_options(
  -Wno-double-promotion
)

zephyr_include_directories(${OPUS_DIR}/include)
zephyr_library_include_directories(
  ${OPUS_DIR}/celt

  ${OPUS_DIR}/silk
  ${OPUS_DIR}/silk/fixed

  ${OPUS_DIR}/src
)

# The source lists of the upstream build: generic C, fixed-point SILK
include(${OPUS_DIR}/cmake/OpusFunctions.cmake)
get_opus_sources(CELT_SOURCES ${OPUS_DIR}/celt_sources.mk celt_sources)
get_opus_sources(SILK_SOURCES ${OPUS_DIR}/silk_sources.mk silk_sources)
get_opus_sources(SILK_SOURCES_FIXED ${OPUS_DIR}/silk_sources.mk silk_sources_fixed)
get_opus_sources(OPUS_SOURCES ${OPUS_DIR}/opus_sources.mk opus_sources)

set(sources ${celt_sources} ${silk_sources} ${silk_sources_fixed} ${opus_sources})
list(TRANSFORM sources PREPEND ${OPUS_DIR}/)
zephyr_library_sources(${sources})
