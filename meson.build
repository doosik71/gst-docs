project('GStreamer manuals and tutorials', 'c',
  version: '1.17.0.1',
  meson_version: '>=0.48.0')

hotdoc = import('hotdoc')
hotdoc_subprojects = []

apiversion = '1.0'
if host_machine.system() == 'windows'
  pathsep = ';'
else
  pathsep = ':'
endif

libs = ''
plugins_doc = ''
deps = []
plugins_sitemap = ''
if get_option('built_subprojects') != ''
    foreach project_name: get_option('built_subprojects').split(',')
        sub = subproject(project_name)
        if sub.get_variable('build_hotdoc')
            message('Building @0@ documentation'.format(project_name))

            foreach lib: sub.get_variable('libs_doc')
              hotdoc_subprojects += [lib]
              libs += lib.full_path() + pathsep
              deps += [lib]
            endforeach

            foreach plugin_doc: sub.get_variable('plugins_doc')
              plugins_doc += plugin_doc.full_path() + pathsep
              hotdoc_subprojects += [plugin_doc]
              deps += [plugin_doc]
            endforeach
        else
            message('@0@ did not build hotdoc documentation, can\'t build API doc'.format(project_name))
        endif
    endforeach
endif
deps += [dependency('gstreamer-' + apiversion, fallback: ['gstreamer', 'gst_dep'])]

if get_option('use_portal_index')
  index = 'markdown/index.md'
else
  index = 'markdown/simple-index.md'
endif
sitemap_gen = find_program('scripts/generate_sitemap.py')
sitemap = configure_file(command: [sitemap_gen, '@INPUT@', '@OUTPUT@',
  'gi-index', libs, plugins_doc],
  input: 'sitemap.txt',
  output: 'sitemap.txt')

html_theme = 'https://github.com/hotdoc/hotdoc_lumen_theme/releases/download/0.999/hotdoc_lumen_theme-0.999.tar.xz?sha256=61088e983e7a5d171f2afc4e270c82f1ca251e38e375658ef230666691418629'

gstreamer_doc = hotdoc.generate_doc('GStreamer',
    project_version: apiversion,
    sitemap: sitemap,
    index: index,
    gi_index: index,
    install: true,
    extra_assets: [join_paths(meson.current_source_dir(), 'images')],
    syntax_highlighting_activate: true,
    html_theme: html_theme,
    include_paths: join_paths(meson.current_source_dir(), 'examples'),
    html_extra_theme: join_paths(meson.current_source_dir(), 'theme/extra'),
    dependencies: deps,
    subprojects: hotdoc_subprojects,
    disable_incremental_build: true,
    gst_list_plugins_page: 'gst-index',
    gst_index: join_paths(meson.current_source_dir(), 'markdown/plugins_doc.md'),
    devhelp_activate: true,
    devhelp_online: 'https://gstreamer.freedesktop.org/documentation/',
    build_always_stale: true,
    edit_on_github_repository: 'https://gitlab.freedesktop.org/gstreamer/gst-docs/',
)

cdata = configuration_data()
cdata.set('GST_API_VERSION', apiversion)
readme = configure_file(input: 'scripts/RELEASE_README.md',
               output: 'README.md',
               configuration : cdata)

run_target('release',
  command: [find_program('scripts/release.py'),
            gstreamer_doc.full_path(),
            'GStreamer-doc-@0@.tar.xz'.format(meson.project_version()),
            join_paths(meson.current_build_dir(), 'README.md')],
  depends: [gstreamer_doc]
)
