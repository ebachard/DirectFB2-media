#  This file is part of DirectFB.
#
#  This library is free software; you can redistribute it and/or
#  modify it under the terms of the GNU Lesser General Public
#  License as published by the Free Software Foundation; either
#  version 2.1 of the License, or (at your option) any later version.
#
#  This library is distributed in the hope that it will be useful,
#  but WITHOUT ANY WARRANTY; without even the implied warranty of
#  MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the GNU
#  Lesser General Public License for more details.
#
#  You should have received a copy of the GNU Lesser General Public
#  License along with this library; if not, write to the Free Software
#  Foundation, Inc., 51 Franklin Street, Fifth Floor, Boston, MA 02110-1301, USA

project('DirectFB2-media', 'c')

directfb_dep = dependency('directfb-internal')

moduledir = directfb_dep.get_pkgconfig_variable('moduledir')

config_h = configuration_data()

config_h.set('WORDS_BIGENDIAN', host_machine.endian() == 'big', description: 'Byte ordering is bigendian.')

fusionsound_dep = []
if get_option('fusionsound')
  fusionsound_dep = dependency('fusionsound', required: false)
  if fusionsound_dep.found()
    config_h.set('HAVE_FUSIONSOUND', 1, description: 'Define to 1 if you are compiling FusionSound support.')
  endif
endif

configure_file(configuration: config_h, output: 'config.h')

config_inc = include_directories('.')

enable_ffmpeg = get_option('ffmpeg')
enable_ft2    = get_option('ft2')
enable_jpeg   = get_option('jpeg')
enable_png    = get_option('png')

if enable_ffmpeg
  ffmpeg_dep = [dependency('libavcodec',  required: false),
                dependency('libavformat', required: false),
                dependency('libavutil',   required: false),
                dependency('libswscale',  required: false)]

  foreach dep : ffmpeg_dep
    if not dep.found()
      warning('FFmpeg video provider will not be built.')
      enable_ffmpeg = false
      break
    endif
  endforeach
endif

if enable_ft2
  ft2_dep = [dependency('freetype2', required: false),
             meson.get_compiler('c').find_library('m')]

  foreach dep : ft2_dep
    if not dep.found()
      warning('FreeType2 font provider will not be built.')
      enable_ft2 = false
      break
    endif
  endforeach
endif

if enable_jpeg
  jpeg_dep = dependency('libjpeg', required: false)

  if not jpeg_dep.found()
    if meson.get_compiler('c').has_header('jpeglib.h')
      jpeg_dep = meson.get_compiler('c').find_library('jpeg', required: false)
      if not jpeg_dep.found()
        warning('JPEG image provider will not be built.')
        enable_jpeg = false
      endif
    endif
  endif
endif

if enable_png
  png_dep = dependency('libpng', required: false)

  if not png_dep.found()
    warning('PNG image provider will not be built.')
    enable_png = false
  endif
endif

subdir('interfaces/IDirectFBFont')
subdir('interfaces/IDirectFBImageProvider')
subdir('interfaces/IDirectFBVideoProvider')

subdir('tools')
