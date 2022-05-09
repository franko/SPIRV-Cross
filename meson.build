project('SPIRV-Cross', 'cpp', default_options : ['cpp_std=c++11'])

spirv_cross_cppargs = []
if get_option('exceptions-assert')
    # pass to meson also the -Dcpp_eh=none options to disable exceptions
    spirv_cross_cppargs += '-DSPIRV_CROSS_EXCEPTIONS_TO_ASSERTIONS'
endif

spirv_cross_sources = [
    'spirv_cfg.cpp',
    'spirv_cpp.cpp',
    'spirv_cross.cpp',
    'spirv_cross_c.cpp',
    'spirv_cross_parsed_ir.cpp',
    'spirv_cross_util.cpp',
    'spirv_glsl.cpp',
    'spirv_hlsl.cpp',
    'spirv_msl.cpp',
    'spirv_parser.cpp',
    'spirv_reflect.cpp',
]

libspirv_cross = library('spirv-cross', spirv_cross_sources,
    cpp_args : spirv_cross_cppargs
)

if get_option('buildtype') == 'release'
    spirv_cross_cppargs += '-DNDEBUG'
endif

if get_option('cli')
    executable('spirv-cross', 'main.cpp',
        link_with : libspirv_cross,
        cpp_args : spirv_cross_cppargs,
        install: true)
endif

pkg = import('pkgconfig')

pkg.generate(libspirv_cross,
    filebase : 'spirv-cross',
    name : 'spirv-cross',
    description : 'C API for SPIRV-Cross',
    url : 'github.com/KhronosGroup/SPIRV-Cross',
)

