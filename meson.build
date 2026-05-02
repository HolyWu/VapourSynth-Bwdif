project('Bwdif', 'cpp',
    default_options: ['buildtype=release', 'warning_level=2', 'b_lto=true', 'b_ndebug=if-release', 'cpp_std=c++17'],
    license: 'LGPL-3.0-or-later',
    license_files: 'LICENSE',
    meson_version: '>=1.2.3',
    version: '5.0',
)

py = import('python').find_installation(pure: false)

incdir = include_directories(
    run_command(py, '-c', 'import vapoursynth as vs; print(vs.get_include())', check: true).stdout().strip(),
)

libs = []

if host_machine.cpu_family().startswith('x86')
    add_project_arguments('-DBWDIF_X86', language: 'cpp')

    libs += static_library('instrset_detect',
        files('Bwdif/vectorclass/instrset_detect.cpp'),
        gnu_symbol_visibility: 'hidden',
    )

    libs += static_library('sse4',
        files('Bwdif/Bwdif_SSE4.cpp'),
        cpp_args: '-march=x86-64-v2',
        gnu_symbol_visibility: 'hidden',
        include_directories: incdir,
    )

    libs += static_library('avx2',
        files('Bwdif/Bwdif_AVX2.cpp'),
        cpp_args: '-march=x86-64-v3',
        gnu_symbol_visibility: 'hidden',
        include_directories: incdir,
    )

    libs += static_library('avx512',
        files('Bwdif/Bwdif_AVX512.cpp'),
        cpp_args: '-march=x86-64-v4',
        gnu_symbol_visibility: 'hidden',
        include_directories: incdir,
    )
endif

shared_module('bwdif',
    files('Bwdif/Bwdif.cpp'),
    gnu_symbol_visibility: 'hidden',
    include_directories: incdir,
    install: true,
    install_dir: py.get_install_dir() / 'vapoursynth/plugins',
    link_with: libs,
    name_prefix: '',
)
