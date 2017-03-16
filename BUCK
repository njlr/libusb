def merge_dicts(x, y):
  z = x.copy()
  z.update(y)
  return z

MACOS_CONFIG_H = """
#define DEFAULT_VISIBILITY __attribute__((visibility(\\"default\\")))
#define ENABLE_LOGGING 1
#define HAVE_DECL_TFD_CLOEXEC 0
#define HAVE_DECL_TFD_NONBLOCK 0
#define HAVE_DLFCN_H 1
#define HAVE_INTTYPES_H 1
#define HAVE_MEMORY_H 1
#define HAVE_POLL_H 1
#define HAVE_STDINT_H 1
#define HAVE_STDLIB_H 1
#define HAVE_STRINGS_H 1
#define HAVE_STRING_H 1
#define HAVE_STRUCT_TIMESPEC 1
#define HAVE_SYS_STAT_H 1
#define HAVE_SYS_TIME_H 1
#define HAVE_SYS_TYPES_H 1
#define HAVE_UNISTD_H 1
#define LT_OBJDIR \\".libs/\\"
#define OS_DARWIN 1
#define PACKAGE \\"libusb\\"
#define PACKAGE_BUGREPORT \\"libusb-devel@lists.sourceforge.net\\"
#define PACKAGE_NAME \\"libusb\\"
#define PACKAGE_STRING \\"libusb 1.0.20\\"
#define PACKAGE_TARNAME \\"libusb\\"
#define PACKAGE_URL \\"http://libusb.info\\"
#define PACKAGE_VERSION \\"1.0.20\\"
#define POLL_NFDS_TYPE nfds_t
#define STDC_HEADERS 1
#define THREADS_POSIX 1
#define VERSION \\"1.0.20\\"
#define _GNU_SOURCE 1
"""

genrule(
  name = 'macos-config.h',
  out = 'config.h',
  cmd = 'echo "' + MACOS_CONFIG_H + '" > $OUT',
)

posix_headers = {
  'os/threads_posix.h': 'libusb/os/threads_posix.h',
  'os/poll_posix.h': 'libusb/os/poll_posix.h',
}

macos_headers = merge_dicts(posix_headers, {
  'config.h': ':macos-config.h',
  'darwin_usb.h': 'libusb/os/darwin_usb.h',
})

posix_sources = [
  'libusb/os/threads_posix.c',
  'libusb/os/poll_posix.c',
]

macos_sources = posix_sources + [
  'libusb/os/darwin_usb.c',
]

platform_sources = macos_sources

macos_exported_linker_flags = [
  '-framework', 'IOKit',
  '-framework', 'CoreFoundation',
]

cxx_library(
  name = 'libusb',
  header_namespace = '',
  exported_headers = subdir_glob([
    ('libusb', '*.h'),
  ]),
  platform_headers = [
    ('default', macos_headers),
    ('^macos.*', macos_headers),
  ],
  srcs = glob([
    'libusb/*.c',
  ]),
  platform_srcs = [
    ('default', macos_sources),
    ('^macos.*', macos_sources),
  ],
  compiler_flags = [
    '-std=c11',
  ],
  exported_platform_linker_flags = [
    ('default', macos_exported_linker_flags),
    ('^macos.*', macos_exported_linker_flags),
  ],
  visibility = [
    'PUBLIC',
  ],
)
