
project('uavcan_tests',
  [ 'c', 'cpp' ],
)



uavcan_dir = 'libuavcan'

# Emulate libuavcan/libuavcan/include.mk
libuavcan_srcdir = join_paths(uavcan_dir, 'libuavcan', 'src')
libuavcan_incdir = join_paths(uavcan_dir, 'libuavcan', 'include')

libuavcan_sources = run_command([
  'find', libuavcan_srcdir, '-type', 'f', '-name', '*.cpp'
]).stdout().strip().split('\n')


# Generate headers from uavcan files
libuavcan_dsdlc = find_program(
  join_paths(uavcan_dir, 'libuavcan', 'dsdl_compiler', 'libuavcan_dsdlc')
)

libuavcan_sources += custom_target('dsdl_sources',
  output: 'Timestamp.hpp', # Known generated source
  command: [
    libuavcan_dsdlc,
    join_paths(meson.current_source_dir(), uavcan_dir, 'dsdl', 'uavcan'),
    '-O', meson.current_build_dir(),
  ]
)

static_library('uavcan',
  libuavcan_sources,
  include_directories: [
    include_directories(libuavcan_incdir),
  ],
  cpp_args: [
    '-DUAVCAN_CPP_VERSION=@0@'.format('UAVCAN_CPP03'),
  ],
)
