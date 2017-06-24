#
# CMakeLists.txt -- build system for LuaSDL2
#
# Copyright (c) 2013, 2014 David Demelier <markand@malikania.fr>
# Copyright (c) 2014, 2015 Joseph Wallace <tangent128@gmail.com>
# Copyright (c) 2017 Webster Sheets <webster@web-eworks.com>
#
# Permission to use, copy, modify, and/or distribute this software for any
# purpose with or without fee is hereby granted, provided that the above
# copyright notice and this permission notice appear in all copies.
#
# THE SOFTWARE IS PROVIDED "AS IS" AND THE AUTHOR DISCLAIMS ALL WARRANTIES
# WITH REGARD TO THIS SOFTWARE INCLUDING ALL IMPLIED WARRANTIES OF
# MERCHANTABILITY AND FITNESS. IN NO EVENT SHALL THE AUTHOR BE LIABLE FOR
# ANY SPECIAL, DIRECT, INDIRECT, OR CONSEQUENTIAL DAMAGES OR ANY DAMAGES
# WHATSOEVER RESULTING FROM LOSS OF USE, DATA OR PROFITS, WHETHER IN AN
# ACTION OF CONTRACT, NEGLIGENCE OR OTHER TORTIOUS ACTION, ARISING OUT OF
# OR IN CONNECTION WITH THE USE OR PERFORMANCE OF THIS SOFTWARE.
#

cmake_minimum_required(VERSION 2.8)

# Project information.  Follows the [SEMVER](semver.org) standard
set(BINDING_MAJOR "2")
set(BINDING_MINOR "1")

# Latest implemented SDL2 version.  Only increment when a new SDL2 version is
# completely implemented.
set(MAJOR "2")
set(MINOR "0")
set(PATCH "5")

set(VERSION "${BINDING_MAJOR}.${BINDING_MINOR} for SDL ${MAJOR}.${MINOR}.${PATCH}")

# Lua version: JIT, 51, 52, and 53, default: 5.2
set(WITH_LUAVER "53"
    CACHE STRING "Version of Lua to use")

set(
	SOURCES
	src/audio.c
	src/audio.h
	src/channel.c
	src/channel.h
	src/clipboard.c
	src/clipboard.h
	src/cpu.c
	src/cpu.h
	src/display.c
	src/display.h
	src/events.c
	src/events.h
	src/filesystem.c
	src/filesystem.h
	src/gamecontroller.c
	src/gamecontroller.h
	src/gl.c
	src/gl.h
	src/haptic.c
	src/haptic.h
	src/joystick.c
	src/joystick.h
	src/keyboard.c
	src/keyboard.h
	src/logging.c
	src/logging.h
	src/mouse.c
	src/mouse.h
	src/platform.c
	src/platform.h
	src/power.c
	src/power.h
	src/rectangle.c
	src/rectangle.h
	src/renderer.c
	src/renderer.h
	src/SDL.c
	src/texture.c
	src/texture.h
	src/thread.c
	src/thread.h
	src/timer.c
	src/timer.h
	src/window.c
	src/window.h
)

configure_file(
	${CMAKE_CURRENT_SOURCE_DIR}/cmake/config.h.in
	${CMAKE_CURRENT_BINARY_DIR}/config.h
)

include_directories(
	${SDL2_INCLUDE_DIR}
	${CMAKE_CURRENT_SOURCE_DIR}
        ${CMAKE_CURRENT_BINARY_DIR}
	${LUA_INCLUDE_DIR}
)

#find_package(SysQueue)
#if (NOT SYSQUEUE_FOUND
#    OR NOT SYSQUEUE_SLIST_FOREACH_SAFE
#    OR NOT SYSQUEUE_STAILQ_FOREACH_SAFE
#    OR NOT SYSQUEUE_LIST_FOREACH_SAFE
#    OR NOT SYSQUEUE_TAILQ_FOREACH_SAFE)
#	include_directories(BEFORE ${Lua-SDL2_SOURCE_DIR}/extern/queue)
#endif ()

# Common code for modules
add_subdirectory(common)

add_library(
	voxsdl
	MODULE
	${SOURCES}
)

set_target_properties(
	voxsdl
	PROPERTIES
	PREFIX ""
)

target_link_libraries(
	voxsdl
	common
)

install(
	TARGETS voxsdl LIBRARY
	DESTINATION share/voxvision/modules
)

# Other libSDL sub modules
#option(WITH_IMAGE "Enable SDL_image" On)
#option(WITH_MIXER "Enable SDL_mixer" On)
#option(WITH_TTF "Enable SDL_ttf" On)
#option(WITH_NET "Enable SDL_net" On)

#if (WITH_IMAGE)
#	add_subdirectory(sdl-image)
#endif ()

#if (WITH_MIXER)
#	add_subdirectory(sdl-mixer)
#endif ()

#if (WITH_TTF)
#	add_subdirectory(sdl-ttf)
#endif ()

#if (WITH_NET)
#	add_subdirectory(sdl-net)
#endif ()

# For the examples and tutorials
#add_subdirectory(examples)
#add_subdirectory(tutorials)
