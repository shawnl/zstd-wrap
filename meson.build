project('zstd', 'c',
  license: 'BSD',
  version: '1.3.2')

libm = meson.get_compiler('c').find_library('m', required: true)

lib_dir = join_paths('lib')
common_dir = join_paths(lib_dir, 'common')
compress_dir = join_paths(lib_dir, 'compress')
decompress_dir = join_paths(lib_dir, 'decompress')
dictbuilder_dir = join_paths(lib_dir, 'dictBuilder')
deprecated_dir = join_paths(lib_dir, 'deprecated')

libzstd_srcs = [
    join_paths(common_dir, 'entropy_common.c'),
    join_paths(common_dir, 'fse_decompress.c'),
    join_paths(common_dir, 'threading.c'),
    join_paths(common_dir, 'pool.c'),
    join_paths(common_dir, 'zstd_common.c'),
    join_paths(common_dir, 'error_private.c'),
    join_paths(common_dir, 'xxhash.c'),
    join_paths(compress_dir, 'fse_compress.c'),
    join_paths(compress_dir, 'huf_compress.c'),
    join_paths(compress_dir, 'zstd_compress.c'),
    join_paths(compress_dir, 'zstd_fast.c'),
    join_paths(compress_dir, 'zstd_double_fast.c'),
    join_paths(compress_dir, 'zstd_lazy.c'),
    join_paths(compress_dir, 'zstd_opt.c'),
    join_paths(compress_dir, 'zstd_ldm.c'),
    join_paths(compress_dir, 'zstdmt_compress.c'),
    join_paths(decompress_dir, 'huf_decompress.c'),
    join_paths(decompress_dir, 'zstd_decompress.c'),
    join_paths(dictbuilder_dir, 'cover.c'),
    join_paths(dictbuilder_dir, 'divsufsort.c'),
    join_paths(dictbuilder_dir, 'zdict.c'),
    join_paths(deprecated_dir, 'zbuff_common.c'),
    join_paths(deprecated_dir, 'zbuff_compress.c'),
    join_paths(deprecated_dir, 'zbuff_decompress.c')
]

libzstd_includes = [include_directories(common_dir, dictbuilder_dir, compress_dir, lib_dir)]

if get_option('legacy_support')
    message('Enabling legacy support')
    libzstd_cflags = ['-DZSTD_LEGACY_SUPPORT=4']

    legacy_dir = join_paths(lib_dir, 'legacy')
    libzstd_includes += [include_directories(legacy_dir)]
    libzstd_srcs += [
        join_paths(legacy_dir, 'zstd_v01.c'),
        join_paths(legacy_dir, 'zstd_v02.c'),
        join_paths(legacy_dir, 'zstd_v03.c'),
        join_paths(legacy_dir, 'zstd_v04.c'),
        join_paths(legacy_dir, 'zstd_v05.c'),
        join_paths(legacy_dir, 'zstd_v06.c'),
        join_paths(legacy_dir, 'zstd_v07.c')
    ]
else
    libzstd_cflags = []
endif

if get_option('multithread')
    message('Enabling multi-threading support')
    add_global_arguments('-DZSTD_MULTITHREAD', language: 'c')
    libzstd_deps = [dependency('threads')]
else
    libzstd_deps = []
endif

libzstd_shared = shared_library('zstd',
                  libzstd_srcs,
                  include_directories: libzstd_includes,
                  c_args: libzstd_cflags,
                  dependencies: libzstd_deps,
                  )

libzstd_static = static_library('zstd',
                  libzstd_srcs,
                  include_directories: libzstd_includes,
                  c_args: libzstd_cflags,
                  dependencies: libzstd_deps,
                  )

libzstd_external_includes = [include_directories(lib_dir, common_dir)]
zstd_shared_dep = declare_dependency(link_with : libzstd_shared, include_directories: libzstd_external_includes)
zstd_static_dep = declare_dependency(link_with : libzstd_static, include_directories: libzstd_external_includes)

