project('SPIRV-Cross', 'cpp', default_options : ['cpp_std=c++11'])

spirv_cross_cppargs = []
if get_option('cpp_eh') == 'none'
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

libspirv_cross = library('spirv-cross', spirv_cross_sources)

if get_option('buildtype') == 'release'
    spirv_cross_cppargs += '-DNDEBUG'
endif

if get_option('cli')
    executable('spirv-cross', 'main.cpp',
        link_with : libspirv_cross,
        install: true)
endif
