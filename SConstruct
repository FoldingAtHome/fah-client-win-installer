# Configure system boilerplate
import os, sys
sys.path.append(os.environ.get('CONFIG_SCRIPTS_HOME',
                               '../../control/config-scripts'))
import config

# Version
version = open('version/version.txt', 'r').read().strip()
major, minor, revision = version.split('.')

# Setup
env = config.make_env(['packager'])

# Configure
conf = Configure(env)

# Packaging
config.configure('packager', conf)
conf.Finish()

# Check and set home variables
for var in 'CLIENT CONTROL VIEWER SCREENSAVER'.split():
    if not var + '_HOME' in os.environ: raise Exception, var + '_HOME not set'
    env[var + '_HOME'] = os.environ.get(var + '_HOME')

# Package

# Code sign key password
path = os.environ.get('CODE_SIGN_KEY_PASS_FILE')
if path is not None:
    code_sign_key_pass = open(path, 'r').read().strip()
else: code_sign_key_pass = None

if 'SIGNTOOL' in os.environ: env['SIGNTOOL'] = os.environ['SIGNTOOL']

pkg = env.Packager(
    'fah-installer',
    version = version,
    url = 'http://folding.stanford.edu/',
    summary = 'Folding@home Client',
    nsi = 'FAHClient.nsi',
    timestamp_url = 'http://timestamp.comodoca.com/authenticode',
    code_sign_key = os.environ.get('CODE_SIGN_KEY', None),
    code_sign_key_pass = code_sign_key_pass,
    )

AlwaysBuild(pkg)
env.Alias('package', pkg)

