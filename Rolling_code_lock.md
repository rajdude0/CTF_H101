# Model E1337 - Rolling Code Lock



1 trial = 48455944, expected = 08922516

2 trial = 08922516 expected = 54002493

3 trial = 54002493 expected = 48743815





```python
### main.py
from flask import Flask, abort, redirect, request, Response, session
from jinja2 import Template
import base64, json, os, random, re, subprocess, time, xml.sax
from cStringIO import StringIO

from rng import *

flags = json.loads(os.getenv('FLAGS'))
os.unsetenv('FLAGS') #---> unset the env so can't read it again. VERY GOOD!

app = Flask(__name__)

templateCache = {}
def render(tpl, **kwargs):
	if tpl not in templateCache:
		templateCache[tpl] = Template(file('templates/%s.html' % tpl).read())
	return templateCache[tpl].render(**kwargs)

@app.after_request
def add_header(r):
	r.headers["Cache-Control"] = "no-cache, no-store, must-revalidate"
	r.headers["Pragma"] = "no-cache"
	r.headers["Expires"] = "0"
	r.headers['Cache-Control'] = 'public, max-age=0'
	return r

@app.route('/')
def index():
	return render('home')

@app.route('/unlock', methods=['POST'])
def unlock():
	code = int(request.form['code'])
	cur = next(26)
	time.sleep(5)

	if code == cur:
		return 'Unlocked successfully.  Flag: ' + flags[1]
	else:
		return 'Code incorrect.  Expected %08i' % cur

@app.route('/admin')
def admin():
	return render('admin', location=location)

location = 'Front door'

@app.route('/get-config')
def getConfig():
	return '<?xml version="1.0" encoding="UTF-8"?><config><location>%s</location></config>' % location

class Handler(xml.sax.ContentHandler):
	def __init__(self):
		self.location = None
	def startElement(self, name, attrs):
		if name == 'location':
			self.location = ''
	def endElement(self, name):
		if name == 'location':
			global location
			location = self.location
			self.location = None
	def characters(self, content):
		if self.location is not None:
			self.location += content

@app.route('/set-config')
def setConfig():
	data = request.args['data']
	parser = xml.sax.make_parser()
	parser.setContentHandler(Handler())
	parser.parse(StringIO(data))
	return redirect('admin')

app.run(host='0.0.0.0', port=80)

```

```python
#### rng.py
def setup(seed):
	global state
	state = 0
	for i in xrange(16):
		cur = seed & 3
		seed >>= 2
		state = (state << 4) | ((state & 3) ^ cur)
		state |= cur << 2

def next(bits):
	global state

	ret = 0
	for i in xrange(bits):
		ret <<= 1
		ret |= state & 1
		state = (state << 1) ^ (state >> 61)
		state &= 0xFFFFFFFFFFFFFFFF
		state ^= 0xFFFFFFFFFFFFFFFF

		for j in xrange(0, 64, 4):
			cur = (state >> j) & 0xF
			cur = (cur >> 3) | ((cur >> 2) & 2) | ((cur << 3) & 8) | ((cur << 2) & 4)
			state ^= cur << j

	return ret

setup((random.randrange(0x10000) << 16) | random.randrange(0x10000))

```

10453198

04066657