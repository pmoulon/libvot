list(APPEND VOT_MATCHING_HEADER )
list(APPEND VOT_MATCHING_SOURCE )

if(LIBVOT_USE_OPENCV)
	list(APPEND VOT_MATCHING_HEADER opencv_matching_api.h)
	list(APPEND VOT_MATCHING_SOURCE opencv_matching_api.cpp)

  if(LIBVOT_BUILD_SHARED)
      add_library(vot_matching SHARED ${VOT_MATCHING_HEADER} ${VOT_MATCHING_SOURCE})
  else(LIBVOT_BUILD_SHARED)
      add_library(vot_matching STATIC ${VOT_MATCHING_HEADER} ${VOT_MATCHING_SOURCE})
  endif(LIBVOT_BUILD_SHARED)

  target_link_libraries(vot_matching ${OpenCV_LIBS})

  set_target_properties(vot_matching PROPERTIES SOVERSION ${LIBVOT_VERSION_MAJOR}
  VERSION "${LIBVOT_VERSION_MAJOR}.${LIBVOT_VERSION_MINOR}")

  install(TARGETS vot_matching DESTINATION lib CONFIGURATIONS Release)
endif(LIBVOT_USE_OPENCV)
