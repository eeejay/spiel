gdbus_codegen_cmd = find_program('gdbus-codegen')
spiel_iface_docs = custom_target('dbus-docs',
  output : 'temp-org.freedesktop.Speech.Provider.md',
  input : join_paths(interface_src_dir, 'org.freedesktop.Speech.Provider.xml'),
  command : [gdbus_codegen_cmd, '--generate-md', 'temp',
             '--output-directory', meson.current_build_dir(),
             join_paths(interface_src_dir, 'org.freedesktop.Speech.Provider.xml')])

overview_doc = configure_file(input: 'overview.md', output: 'overview.md', copy: true)
urlmap_doc = configure_file(input: 'urlmap.js', output: 'urlmap.js', copy: true)

toml_conf = configuration_data()
toml_conf.set('version', meson.project_version())
toml_conf.set('dbus_doc', spiel_iface_docs.full_path())

message('salaam', spiel_iface_docs.full_path())
# toml_conf.set('dbus_doc', spiel_iface_docs.full_path())
gidocgen = find_program('gi-docgen', required: true)

gidocgen_common_args = [
  '--quiet',
  '--no-namespace-dir',
]

if get_option('werror')
  gidocgen_common_args += ['--fatal-warnings']
endif

# docs_dir = datadir / 'doc'

source_toml = configure_file(
  input: 'Spiel.toml.in',
  output: 'Spiel.toml',
  configuration: toml_conf,
  # install: true,
  # install_dir: docs_dir / 'libspiel',
)

custom_target('libspiel-doc',
  input: [ source_toml, spiel_gir[0] ],
  output: 'libspiel',
  command: [
    gidocgen,
    'generate',
    gidocgen_common_args,
    '--config=@INPUT0@',
    '--output-dir=@OUTPUT@',
    '--content-dir=@0@'.format(meson.current_build_dir()),
    '@INPUT1@',
  ],
  build_by_default: true,
  depends: [ spiel_iface_docs ],
  depend_files: [ overview_doc, urlmap_doc ]
  # install: true,
  # install_dir: docs_dir,
)
