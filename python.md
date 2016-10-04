# Create virtual env
virtualenv virt_env/virt1
virtualenv --no-site-packages 

# Activate env
source virt_env/virt1/bin/activate

# Deactivate
deactivate

http://stackoverflow.com/questions/582336/how-can-you-profile-a-python-script

# List installed moudules
help('modules')

# Anaconda
# http://conda.pydata.org/docs/examples/info.html
conda -h  # help
conda info --all  # View PYTHONPATH

# Enable tab auto completion in python interpreter
import rlcompleter, readline
readline.parse_and_bind('tab:complete')

# Error: Python Version 2.7 required which was not found in the registry
HKEY_LOCAL_MACHINE\SOFTWARE\Python\PythonCore\<python version>\InstallPath

# How to list object's methods
[method for method in dir(object) if callable(getattr(object, method))]

# Python String format example
>>> '{:*^30}'.format('centered')  # use '*' as a fill char
'***********centered***********'

# Print number with thousand separator
'{:,}'.format(value)
# Print integer
'{:d}'.format(value)

# Pretty print dictionary
import pprint
pp = pprint.PrettyPrinter(indent=2)
pp.pprint(myDictInstance)  # If it is a class obj, can try pp.pprint(myObj.__dict__)

# Anaconda environment
# http://conda.pydata.org/docs/examples/create.html
conda create -p ~/anaconda/envs/test2 anaconda=1.4.0 python=2.7 numpy=1.6
# View all environments
conda info --envscd 

# pip
pip freeze  # View installed packages
pip install [-e, --editable] <path/url>  # Similar to setup.py develop

# Retrieve environment variable
os.getenv('PATH')  # Returns None if nothing
os.getenv('PATH', 'default')

# Add path to python path in code
import sys
sys.path.append('/path/to/module')

# Find module location
import mymod
mymode.__file__

# Change matplotlib graph size
from pylab import rcParams
rcParams['figure.figsize'] = 5, 10

# Get file name from path
basename = os.path.basename('C:/data/Auditor.csv')
# Get file name and extension
os.path.splitext(basename)  # Returns [file name, extension]

# Read csv
import csv
with open('eggs.csv', 'rb') as csvfile:
    reader = csv.reader(csvfile, delimiter=' ', quotechar='|')
    next(reader, None)  # skip the headers
    for row in reader:
        print ', '.join(row)

    # To read content to list
    my_list = list(reader)

# Using DictReader
with open('names.csv') as csvfile:
    reader = csv.DictReader(csvfile)
    for row in reader:
        print(row['first_name'], row['last_name'])

# Read csv file that is BOM encoded
# http://stackoverflow.com/questions/2359832/dealing-with-utf-8-numbers-in-python
import codecs

with codecs.open(file, "r", "utf-8-sig") as f:
    a, b, c= map(int, f.readline().split(","))
        
# Write csv from dict
with open('names.csv', 'w') as csvfile:
    fieldnames = ['first_name', 'last_name']
    writer = csv.DictWriter(csvfile, fieldnames=fieldnames)

    writer.writeheader()
    writer.writerow({'first_name': 'Baked', 'last_name': 'Beans'})
    writer.writerow({'first_name': 'Lovely', 'last_name': 'Spam'})
    writer.writerow({'first_name': 'Wonderful', 'last_name': 'Spam'})

# List comprehension with if and if-else
[y for y in a if y not in b]
[y if y not in b else other_value for y in a]

# Nested list comprehension order
# http://stackoverflow.com/questions/8049798/understanding-nested-list-comprehension
[item for row in matrix for item in row] # nesting is in left-to-right order

# Dictionary comprehension
d = {k:v for k, v in iterable}
d = {n: n**2 for n in range(5)}
d = {n: True for n in range(5)}
dict.fromkeys(range(1, 11), True)
d = dict.fromkeys(range(10), [])  # CAREFUL, all key contains the same [] ref

# Dictionary initialization
a = { 'import': 'trade', 1: 7.8 }

# Print object properties
# http://stackoverflow.com/questions/5969806/print-all-properties-of-a-python-class
vars(object)

# Check if object has property/attribute
hasattr(myobj, 'prop')
# Better to just use
getattr(myobj, 'prop')

# Get user directory
os.path.expanduser('~')

# iPython: View all user defined variables
who
whos  # more info

# Function decorator @wraps
http://stackoverflow.com/questions/308999/what-does-functools-wraps-do

# Uninstall package from develop
python setup.py develop --uninstall

# Future print
from __future__ import print_function

# Print without u'
unicode('a')

# Deep copy object
# http://stackoverflow.com/questions/4794244/how-can-i-create-a-copy-of-an-object-in-python
clone = copy.deepcopy(original)

# Select random value from list
# http://stackoverflow.com/questions/306400/how-do-i-randomly-select-an-item-from-a-list-using-python
import random
foo = ['a', 'b', 'c', 'd', 'e']
print(random.choice(foo))
# With no duplicate
random.sample(range(100), 10)

# Install opencv python linux, build from source
# http://docs.opencv.org/master/dd/dd5/tutorial_py_setup_in_fedora.html#gsc.tab=0
sudo dnf -y install gtk2-devel libdc1394-devel libv4l-devel ffmpeg-devel gstreamer-plugins-base-devel libpng-devel tbb-devel eigen3-devel bzip2-devel
cmake -D CMAKE_BUILD_TYPE=RELEASE -D CMAKE_INSTALL_PREFIX=/usr/local -D WITH_TBB=ON -D WITH_EIGEN=ON ..
# May need to do this cuz make look for so.8 version
sudo ln -s /home/davidheryanto/anaconda2/lib/libjpeg.so.8 /usr/lib64/libjpeg.so.8
# site-packages location: /home/davidheryanto/anaconda2/lib/python2.7/site-packages
# e.g. can do:
  sudo ln -s /usr/local/lib/python2.7/site-packages/cv2.so /home/davidheryanto/anaconda2/lib/python2.7/site-packages/cv2.so

# Convert BGR to RGB using opencv
import cv2
srcBGR = cv2.imread("sample.png")
destRGB = cv2.cvtColor(srcBGR,COLOR_BGR2RGB)
# Using PIL Image
# http://stackoverflow.com/questions/4661557/pil-rotate-image-colors-bgr-rgbdata = np.asarray(im)
im = Image.fromarray(np.roll(data, 1, axis=-1))

# Get home directory
os.path.expanduser('~') 

# List all files in a directory
import glob
print glob.glob("/home/adam/*.txt")

from os import listdir
from os.path import isfile, join
onlyfiles = [ f for f in listdir(mypath) if isfile(join(mypath,f)) ]

# Convert/parse string to dict/list
# http://stackoverflow.com/questions/988228/converting-a-string-to-dictionary
import ast
ast.literal_eval("{'muffin' : 'lolz', 'foo' : 'kitty'}")

# Fast xml parser
# http://effbot.org/zone/celementtree.htm
import xml.etree.cElementTree as ElementTree

# Read gzip
import gzip
with gzip.open('file.txt.gz', 'rb') as f:
    file_content = f.read()

# Write gzip
import gzip
content = "Lots of content here"
with gzip.open('file.txt.gz', 'wb') as f:
    f.write(content)

# Python multiline
s = """ this is a very
        long string if I had the
        energy to type more and more ..."""
s = ("this is a very"
      "long string too"
      "for sure ..."
     )

# namedtuple example
from collections import namedtuple
Point = namedtuple('Point', ['x', 'y'], verbose=True)

# Regex group example
import re 
a = 'tfo.asiainsider_transaction_code.out'
m = re.search('tfo\.(.*)\.out',a)\
print(m.group(1))

# Regex examples
# http://www.thegeekstuff.com/2014/07/python-regex-examples/
re.findall(r'^\d{4}[a-zA-Z].*[a-zA-Z]$','2192dafdf')  # First 4 char must be number and last char is alpha

# Sort list of dict
sorted(listofdict, key=lambda mydict:mydict['key'])

from operator import itemgetter
sorted(listofdict, key=itemgetter('key'))

# Sort dict by key or value, x[0] or x[1] in the key
d = sorted(d.items(), key=lambda x:x[0])

# Joblib recipe
# Use grouper: https://docs.python.org/2/library/itertools.html#recipes
def grouper(n, iterable, fillvalue=None):
    "grouper(3, 'ABCDEFG', 'x') --> ABC DEF Gxx"
    args = [iter(iterable)] * n
    return izip_longest(fillvalue=fillvalue, *args)
for item1, item2 in grouper(2, l):
    # Do something with item1 and item2

chunks = grouper(job_count, geninfo_paths, fillvalue=None)
Parallel(n_jobs=job_count)(delayed(insert_geninfo)(fp, i * 4 + j)
                                   for i, chunk in enumerate(chunks)
                                   for j, fp in enumerate(chunk))

# Logging
import logging
logging.basicConfig(level=logging.INFO)
logger = logging.getLogger(__name__)
logger.info("Hello from info")
logger.debug("Hello from debug")

# Read config file
# [your-config]
# path1 = "D:\test1\first"
# path2 = "D:\test2\second"
import ConfigParser
configParser = ConfigParser.RawConfigParser()   
configFilePath = 'c:/abc.txt'
configParser.read(configFilePath)
configParser.get('your-config', 'path1')

# Change logging level dynamically
# http://stackoverflow.com/questions/19617355/dynamically-changing-log-level-in-python-without-restarting-the-application
logging.getLogger().setLevel(logging.DEBUG)

# Logging across multiple modules
http://stackoverflow.com/questions/16947234/python-logging-across-multiple-modules

# Python print local time in readable format
import time
time.asctime()

# Convert string to datetime with strptime
# https://docs.python.org/2/library/datetime.html#datetime.datetime.strptime
datetime.datetime.strptime('1980-10-25 16:23:12', '%Y-%m-%d %H:%M:%S')
# Convert datetime to string with strftime
# Format spec: https://docs.python.org/2/library/datetime.html#strftime-strptime-behavior
datetime.datetime.now().strftime('%Y-%m-%d')

# Datetime manipulation
d = date.today() - timedelta(days=days_to_subtract)

# Convert iso8601 to datetime
import dateutil.parser
yourdate = dateutil.parser.parse(datestring)

# datetime to iso8601
mydatetime.isoformat()

# Get epoch time
datetime.datetime.utcfromtimestamp(0)

# Timestamp to datetime
from datetime import datetime
datetime.fromtimestamp(1172969203.1)  # Not timezone aware
datetime.datetime.fromtimestamp(1172969203.1).replace(tzinfo=dateutil.tz.tzutc())

# Datetime to timestamp
# Assume datetime is tz aware with tzinfo=tzutc()  -- tzutc from dateutil.tz
def get_posix_timestamp(datetime_instance):
    return (datetime_instance - datetime.utcfromtimestamp(0).replace(tzinfo=dateutil.tz.tzutc())).total_seconds()

# timestamp in Singapore Timezone
def get_posix_timestamp(datetime_instance):
    import dateutil
    return (datetime_instance - timedelta(hours=8) - datetime.utcfromtimestamp(0)).total_seconds()

# Group item by time interval
# http://stackoverflow.com/questions/8825969/grouping-messages-by-time-intervals
interval = 60
[(k * interval, list(g)) for k, g in groupby(sorted_query_entities, lambda x: int(get_posix_timestamp(x['Timestamp'])) / interval)] 

# Regex: remove text within parentheses
re.sub(r'\([^)]*\)', '', text)

# Complexity of data structures
https://www.ics.uci.edu/~pattis/ICS-33/lectures/complexitypython.txt

# Conda SSL Error, modify request to use private cert bundle
# http://stackoverflow.com/questions/31729076/conda-ssl-error
export REQUESTS_CA_BUNDLE=/usr/local/share/ca-certificates/<my-cert-name>

# Disable SSL certificate check when using httplib2
import httplib2
http = httplib2.Http(disable_ssl_certificate_validation=True)

# Unit test nosetest: skip testing method
# http://stackoverflow.com/questions/1120148/disabling-python-nosetests
@nottest

# Prevent nosetests from capturing output
# http://stackoverflow.com/questions/5975194/nosetests-is-capturing-the-output-of-my-print-statements-how-to-circumvent-this
nosetests -s mytest.py

# Create new directory if not exists
# http://stackoverflow.com/questions/273192/in-python-check-if-a-directory-exists-and-create-it-if-necessary
# Python 2.7
try: 
    os.makedirs(path)
except OSError:
    if not os.path.isdir(path):
        raise
# Python 3.4
os.makedirs(path, exist_ok=True)

# Rename files
for fn in glob.glob('./*'):
    os.rename(fn, 'prefix_{}'.format(fn))

# Get memory usage
import os
import psutil
process = psutil.Process(os.getpid())
print('Memory usage: {:,} MB'.format(process.memory_info().rss / 1000))

# pip install memory_profiler
@profile
def f():
    pass

# pip install line_profiler, remember to add @profile to func
kernprof -lv script_to_profile.py

# Call commands (like in bash)
# http://stackoverflow.com/questions/89228/calling-an-external-command-in-python
import subprocess
subprocess.call(['ls', '-l'])

# Extract microsoft word table
# http://stackoverflow.com/questions/10366596/how-to-read-contents-of-an-table-in-ms-word-file-using-python
from docx import Document
wordDoc = Document('<path to docx file>')

for table in wordDoc.tables:
    for row in table.rows:
        for cell in row.cells:
            print cell.text

# Collapse whitespace
return ' '.join(string.split())

# Remove punctuation
''.join(ch for ch in string if ch not in string.punctuation)

# Count generator/iterator
sum(1 for i in it)

# Flush output
sys.stdout.flush()

# run python and disable output buffering, sometimes needed for output in nohup to come out
# http://stackoverflow.com/questions/12919980/nohup-is-not-writing-log-to-output-file
nohup python -u ./script.py &> nohup.out &

# String Formatting: Make CamelCase
'make IT camel CaSe'.title().replace(' ', '')

# Nice packages
# ==============
- cryptography
- passlib