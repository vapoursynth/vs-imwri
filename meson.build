project('IMWRI', 'cpp',
  default_options: ['buildtype=release', 'warning_level=2', 'b_lto=true', 'b_ndebug=if-release', 'cpp_std=c++14'],
  license: 'LGPL-2.1-or-later',
  meson_version: '>=0.51.0',
  version: '1'
)

add_project_arguments('-Wno-unused-parameter', language: 'cpp')

vapoursynth_dep = dependency('vapoursynth').partial_dependency(compile_args: true, includes: true)

deps = [
  vapoursynth_dep,
  dependency('libheif', static: get_option('static')),
  dependency('libtiff-4', static: get_option('static')),
  dependency('Magick++', static: get_option('static'))
]

install_dir = vapoursynth_dep.get_variable(pkgconfig: 'libdir') / 'vapoursynth'

sources = [
  'src/imwri.cpp',
  'src/vsutf16.h'
]

if host_machine.cpu_family().startswith('x86')
  add_project_arguments('-mfpmath=sse', '-msse2', language: 'cpp')
endif

if host_machine.system() == 'windows'
  add_project_link_arguments('-lws2_32', language: 'cpp')
endif

if get_option('static')
  add_project_link_arguments('-static', language: 'cpp')
endif

shared_module('imwri', sources,
  dependencies: deps,
  install: true,
  install_dir: install_dir,
  gnu_symbol_visibility: 'hidden'
)
