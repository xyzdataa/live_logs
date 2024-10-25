#!/usr/bin/env python
'''
5 DEC 13
KernelSanders
Version 1.0
Python 2.7
'''

from __future__ import print_function
import urllib, urllib2, cookielib

MAC = 'XX:XX:XX:XX:XX:XX'
REQUESTED_IP = '10.59.0.191'

print("Logging in to IHG", end='')

cookie_jar = cookielib.CookieJar()
opener = urllib2.build_opener(urllib2.HTTPCookieProcessor(cookie_jar))
urllib2.install_opener(opener)

# log out in order to reset our 'time left' to 24 full hours
urllib2.urlopen("http://rap.nnu.com/logout").read()

# acquire cookie
url_1 = 'https://rap.nnu.com/login'
req = urllib2.Request(url_1)
rsp = urllib2.urlopen(req)

# status output
print(".", end='')

# do first POST
url_2 = 'http://redir.nnu.com/post/'
values = dict(w='1920', h='1080', url='', 
	loginpageurl='https://rap.nnu.com/login', hwid=MAC,
	sysid='32225', usermac='EC:1A:59:88:8D:95', clientip=REQUESTED_IP)
data = urllib.urlencode(values)
req = urllib2.Request(url_2, data)
rsp = urllib2.urlopen(req)
content = rsp.read()

print(".", end='')

# do first GET (not sure you need this, but o well)
urllib2.urlopen("https://portal.nnu.com/templatet/?sysid=32225&\
	usermac=" + MAC + "&w=1920&loginpageurl=https://rap.nnu.com/login\
	&h=1080&clientip=" + REQUESTED_IP).read()

# do second POST
url_3 = 'https://portal.nnu.com/templatet/login'
values = dict(logintype='promo', email='', promocode='NhAv59UtTA', freeterms='on')
data = urllib.urlencode(values)
req = urllib2.Request(url_3, data)
rsp = urllib2.urlopen(req)
content = rsp.read()

print(".", end='')

# do third post POST (haha good username...)
url_4 = 'https://rap.nnu.com/login'
values = dict(username='nnupromo2/NhAv59UtTA/52239741', password=']S*}*VIUZEILKFW-', popup='true')
data = urllib.urlencode(values)
req = urllib2.Request(url_4, data)
rsp = urllib2.urlopen(req)
content = rsp.read()

# do another GET (again maybe unnecessary)
urllib2.urlopen("http://redir.nnu.com/?hwid=" + MAC + "&\
	clientip=" + REQUESTED_IP + "&page=loginsuccess").read()

print(".", end='')

# do the final post POST
url_5 = 'http://redir.nnu.com/post/'
values = dict(w='1920', h='1080', sysid='32225',
	clientip=REQUESTED_IP, hwid=MAC,
	page='loginsuccess')
data = urllib.urlencode(values)
req = urllib2.Request(url_5, data)
rsp = urllib2.urlopen(req)
content = rsp.read()

# do the final GET (almost positive you don't need this)
urllib2.urlopen("https://portal.nnu.com/templatet/?h=1080&\
	clientip=" + REQUESTED_IP + "&page=loginsuccess&w=1920&sysid=32225").read()

print("Done.", end='')
