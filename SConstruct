# Setup
import os
env = Environment(ENV = os.environ)
try:
    env.Tool('config', toolpath = [os.environ.get('CBANG_HOME')])
except Exception, e:
    raise Exception, 'CBANG_HOME not set?\n' + str(e)
env.CBLoadTools('packager fah-client-version')
conf = env.CBConfigure()

# Version
version = env.FAHClientVersion()

# Check and set home variables
for var in 'FAH_CLIENT FAH_CONTROL FAH_VIEWER FAH_SCREENSAVER'.split():
    if not var + '_HOME' in os.environ: raise Exception, var + '_HOME not set'
    home = os.environ.get(var + '_HOME')
    if env['PLATFORM'] == 'win32': home = home.replace('/', '\\')
    env[var + '_HOME'] = home

# Code sign key password
path = os.environ.get('CODE_SIGN_KEY_PASS_FILE')
if path is not None:
    code_sign_key_pass = open(path, 'r').read().strip()
else: code_sign_key_pass = None

if 'SIGNTOOL' in os.environ: env['SIGNTOOL'] = os.environ['SIGNTOOL']

# Installer deps
deps = '''
  ${FAH_CLIENT_HOME}/FAHClient.exe
  ${FAH_CLIENT_HOME}/FAHCoreWrapper.exe
  ${FAH_CLIENT_HOME}/HideConsole.exe
  ${FAH_VIEWER_HOME}/FAHViewer.exe
  ${FAH_SCREENSAVER_HOME}/FAHScreensaver.scr
  ${FAH_CONTROL_HOME}/gui/*.exe
'''.split()

import fnmatch
for root, dirnames, filenames in os.walk(env.get('FAH_CONTROL_HOME') + '/gui'):
    for filename in fnmatch.filter(filenames, '*.pyd'):
        deps.append(os.path.join(root, filename))

# Package
pkg = env.Packager(
    'fah-installer',
    version = version,
    url = 'http://folding.stanford.edu/',
    summary = 'Folding@home Client',
    nsi = 'FAHClient.nsi',
    nsi_dll_deps = deps,
    timestamp_url = 'http://timestamp.comodoca.com/authenticode',
    code_sign_key = os.environ.get('CODE_SIGN_KEY', None),
    code_sign_key_pass = code_sign_key_pass,
    )

AlwaysBuild(pkg)
env.Alias('package', pkg)
