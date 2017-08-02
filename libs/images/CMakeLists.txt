add_library(images
  images.c
)

add_custom_command(
  OUTPUT images.c
  DEPENDS images.h images.c_orig
  COMMAND sed -e \"s/^static //\" ${CMAKE_CURRENT_SOURCE_DIR}/images.c_orig
    > images.c
)

add_custom_command(
  OUTPUT images.h
  DEPENDS images.c_orig
  COMMAND sed -e "/^static \\(const char \\*xpm_\\(.\\{1,\\}\\)\\[\\]\\) = {/!d; s//extern \\1;/"
    ${CMAKE_CURRENT_SOURCE_DIR}/images.c_orig
    > images.h
)

add_custom_target(images-header DEPENDS images.h)

set(images_LIBRARIES
  images
  CACHE INTERNAL ""
)

set(images_INCLUDE_DIRS
  "${CMAKE_CURRENT_BINARY_DIR}"
  CACHE INTERNAL ""
)
