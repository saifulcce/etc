# Set url to download package from
-i / --index-url / --extra-index-url

# Allow insecure origin
--trusted-host
pip install --index-url=http://pypi.python.org/simple/ --trusted-host pypi.python.org pandas

# Sample config
# Windows: %APPDATA%\pip\pip.ini
# Linux: $HOME/.config/pip/pip.conf
[global]
index-url = http://pypi.python.org/simple/
trusted-host = pypi.python.org
proxy = http://myproxy.com:8080

# Install from github
pip install git+https://github.com/fail2ban/fail2ban.git

# Install/upgrade without dependencies
pip install --user --upgrade --no-deps git+git://github.com/Theano/Theano.git