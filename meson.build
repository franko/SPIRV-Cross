project('SPIRV-Cross', 'cpp', default_options : ['cpp_std=c++11'])

spirv_cross_cppargs = []
if get_option('exceptions-assert')
    # pass to meson also the -Dcpp_eh=none options to disable exceptions
    spirv_cross_cppargs += '-DSPIRV_CROSS_EXCEPTIONS_TO_ASSERTIONS'
endif

install_headers(
    'GLSL.std.450.h',
    'spirv_common.hpp',
    'spirv_cross_containers.hpp',
    'spirv_cross_error_handling.hpp',
    'spirv.hpp',
    'spirv_cross.hpp',
    'spirv_parser.hpp',
    'spirv_cross_parsed_ir.hpp',
    'spirv_cfg.hpp',
    'spirv_cross_c.h',
    'spirv_glsl.hpp',
    'spirv_cpp.hpp',
    'spirv_msl.hpp',
    'spirv_hlsl.hpp',
    'spirv_reflect.hpp',
    'spirv_cross_util.hpp',
)

spirv_cross_core_sources = [
    'spirv_cross.cpp',
    'spirv_parser.cpp',
    'spirv_cross_parsed_ir.cpp',
    'spirv_cfg.cpp'
]

if get_option('buildtype') == 'release'
    spirv_cross_cppargs += '-DNDEBUG'
endif

libspirv_cross_core = library('spirv-cross-core', spirv_cross_core_sources,
    cpp_args : spirv_cross_cppargs,
    install : true
)

libspirv_cross_glsl = library('spirv-cross-glsl', 'spirv_glsl.cpp',
    link_with : libspirv_cross_core,
    cpp_args : spirv_cross_cppargs,
    install : true
)

libspirv_cross_util = library('spirv-cross-util', 'spirv_cross_util.cpp',
    link_with : libspirv_cross_core,
    cpp_args : spirv_cross_cppargs,
    install : true
)

libspirv_cross_reflect = library('spirv-cross-reflect', 'spirv_reflect.cpp',
    link_with : [libspirv_cross_core, libspirv_cross_glsl],
    cpp_args : spirv_cross_cppargs,
    install : true
)

libspirv_cross_cpp = library('spirv-cross-cpp', 'spirv_cpp.cpp',
    link_with : [libspirv_cross_core, libspirv_cross_glsl],
    cpp_args : spirv_cross_cppargs,
    install : true
)

libspirv_cross_msl = library('spirv-cross-msl', 'spirv_msl.cpp',
    link_with : [libspirv_cross_core, libspirv_cross_glsl],
    cpp_args : spirv_cross_cppargs,
    install : true
)

libspirv_cross_hlsl = library('spirv-cross-hlsl', 'spirv_hlsl.cpp',
    link_with : [libspirv_cross_core, libspirv_cross_glsl],
    cpp_args : spirv_cross_cppargs,
    install : true
)

libspirv_cross_c = library('spirv-cross-c', 'spirv_cross_c.cpp',
    link_with : [libspirv_cross_core, libspirv_cross_glsl, libspirv_cross_hlsl, libspirv_cross_msl, libspirv_cross_reflect],
    cpp_args: spirv_cross_cppargs,
    # We may add the following define but in this case we need to generate gitversion.h
    # + ['-DHAVE_SPIRV_CROSS_GIT_VERSION']
    install : true
)

if get_option('cli')
    executable('spirv-cross', 'main.cpp',
        link_with : [libspirv_cross_core, libspirv_cross_glsl, libspirv_cross_cpp, libspirv_cross_hlsl, libspirv_cross_msl, libspirv_cross_reflect, libspirv_cross_util],
        cpp_args : spirv_cross_cppargs,
        install: true,
    )
endif

spirv_cross_pkgs = [
    { 'lib' : libspirv_cross_core    , 'name' : 'core',    'description' : 'SPIRV-Cross core library' },
    { 'lib' : libspirv_cross_c       , 'name' : 'c',       'description' : 'C API for SPIRV-Cross' },
    { 'lib' : libspirv_cross_glsl    , 'name' : 'glsl',    'description' : 'SPIRV-Cross for GLSL' },
    { 'lib' : libspirv_cross_cpp     , 'name' : 'cpp',     'description' : 'SPIRV-Cross for C++' },
    { 'lib' : libspirv_cross_msl     , 'name' : 'msl',     'description' : 'SPIRV-Cross for MSL' },
    { 'lib' : libspirv_cross_hlsl    , 'name' : 'hlsl',    'description' : 'SPIRV-Cross for HLSL' },
    { 'lib' : libspirv_cross_reflect , 'name' : 'reflect', 'description' : 'SPIRV-Cross reflection API' },
    { 'lib' : libspirv_cross_util    , 'name' : 'util',    'description' : 'SPIRV-Cross utils' },
]

pkg = import('pkgconfig')

foreach spirv_pkg : spirv_cross_pkgs
    pkg.generate(spirv_pkg['lib'],
        filebase : 'spirv-cross-' + spirv_pkg['name'],
        name : 'spirv-cross-' + spirv_pkg['name'],
        description : spirv_pkg['description'],
        url : 'github.com/KhronosGroup/SPIRV-Cross',
    )
endforeach

