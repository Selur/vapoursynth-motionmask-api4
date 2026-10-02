project('MotionMask', 'cpp',
        version: '2',
        default_options: ['cpp_std=c++14', 'buildtype=release'],
        meson_version: '>=0.46')


cxx = meson.get_compiler('cpp')

is_msvc = cxx.get_id() == 'msvc'


warnings = []

if not is_msvc
  warnings += [
    '-Wall',
    '-Wextra',
    '-Wshadow',
  ]
endif

cflags = [
  warnings,
]

ldflags = [
]


host_cpu_family = host_machine.cpu_family()

if host_cpu_family.startswith('x86')
  cflags += '-DMOTIONMASK_X86=1'
  if not is_msvc
    cflags += ['-mfpmath=sse', '-msse2']
  endif
endif


host_system = host_machine.system()

if host_system == 'windows' or host_system == 'cygwin'
  if host_cpu_family == 'x86' and not is_msvc
    cflags += '-mstackrealign'
    ldflags += '-Wl,--kill-at'
  endif
endif


sources = [
  'src/MotionMask.cpp',
]

deps = [
  dependency('vapoursynth', version: '>=55').partial_dependency(includes: true, compile_args: true),
]

shared_module('motionmask',
              sources,
              dependencies: deps,
              link_args: ldflags,
              cpp_args: cflags,
              install: true)
