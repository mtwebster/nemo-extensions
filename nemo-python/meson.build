project('python-nemo',
    'c',
    version : '3.9.0'
)

gnome = import('gnome')

deps = []

python3 = dependency('python3')
nemo = dependency('libnemo-extension', required: true)

deps += python3
deps += nemo
deps += dependency('pygobject-3.0', version: '>=3.0.0')
deps += dependency('gtk+-3.0')
deps += dependency('glib-2.0')

cdata = configuration_data()

version_array = python3.version().split('.')
ret = run_command('python3-config', '--abiflags')

libpath = '@0@/@1@/libpython@2@.@3@@4@.so.1.0'.format(
    get_option('prefix'),
    get_option('libdir'),
    version_array[0],
    version_array[1],
    ret.stdout().strip()
)
datadir = join_paths(get_option('prefix'), get_option('datadir'))

cdata.set_quoted('NEMO_EXTENSION_DIR', nemo.get_pkgconfig_variable('extensiondir'))
cdata.set_quoted('PYTHON_LIBPATH', libpath)
cdata.set('PYGOBJECT_MAJOR_VERSION', 3)
cdata.set('PYGOBJECT_MINOR_VERSION', 0)
cdata.set('PYGOBJECT_MICRO_VERSION', 0)
cdata.set_quoted('DATA_DIR', datadir)

c = configure_file(output : 'config.h',
    configuration : cdata
)

pc_conf = configuration_data()
    pc_conf.set('prefix', get_option('prefix'))
    pc_conf.set('version', meson.project_version())
    configure_file(
    input : 'nemo-python.pc.in',
    output: 'nemo-python.pc',
    configuration: pc_conf,
    install: true,
    install_dir: join_paths(get_option('libdir'), 'pkgconfig'),
)

top_inc = include_directories('.')

subdir('src')

gtkdoc_enabled = get_option('gtk_doc')

if gtkdoc_enabled
  find_program('gtkdoc-scan', required: true)
  subdir('docs')
endif
