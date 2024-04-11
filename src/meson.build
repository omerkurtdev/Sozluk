pkgdatadir = get_option('prefix') / get_option('datadir') / meson.project_name()
moduledir = pkgdatadir / 'sozluk'
gnome = import('gnome')

blueprint_files = files(
    'window.blp')

blueprint_compiler = find_program('blueprint-compiler')
ui_files = []

foreach blueprint_file : blueprint_files
  path_as_string = '@0@'.format(blueprint_file)
  filename = path_as_string.split('/')[-1]

  ui_file = custom_target(path_as_string.underscorify(),
    input: blueprint_file,
    output: filename.replace('.blp', '.ui'),
    command: [blueprint_compiler, 'compile', '--output', '@OUTPUT@', '@INPUT@'])

  ui_files += ui_file
endforeach

gnome.compile_resources('sozluk',
  'sozluk.gresource.xml',
  gresource_bundle: true,
  dependencies: ui_files,
  install: true,
  install_dir: pkgdatadir,
)

python = import('python')

conf = configuration_data()
conf.set('PYTHON', python.find_installation('python3').full_path())
conf.set('VERSION', meson.project_version())
conf.set('localedir', get_option('prefix') / get_option('localedir'))
conf.set('pkgdatadir', pkgdatadir)

configure_file(
  input: 'sozluk.in',
  output: 'sozluk',
  configuration: conf,
  install: true,
  install_dir: get_option('bindir'),
  install_mode: 'r-xr-xr-x'
)

sozluk_sources = [
  '__init__.py',
  'main.py',
  'window.py',
]

install_data(sozluk_sources, install_dir: moduledir)
