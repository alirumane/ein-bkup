---
layout: post
title: "flask cookie and session"
tagline: ""
description: ""
category: 学习笔记
tags: [Flask, Web, Python]
last_updated: 
---


因为HTTP协议是无状态的，即服务器不知道用户上一次做了什么，这严重阻碍了交互式Web应用程序的实现。

## session

	from flask import Flask, render_template_string, \
		session, request, redirect, url_for
	app = Flask(__name__)
	app.secret_key = 'F12Zr47j\3yX R~X@H!jLwf/T'

	@app.route('/')
	def hello_world():
		return 'hello world'

	@app.route('/login')
	def login():
		page = '''
		<form action="{{ url_for('do_login') }}" method="post">
			<p>name: <input type="text" name="user_name" /></p>
			<input type="submit" value="Submit" />
		</form>
		'''
		return render_template_string(page)
		
	@app.route('/do_login', methods=['POST'])
	def do_login():
		name = request.form.get('user_name')
		session['user_name'] = name
		return 'success'

	@app.route('/show')
	def show():
		return session['user_name']

	@app.route('/logout')
	def logout():
		session.pop('user_name', None)
		return redirect(url_for('login'))

	if __name__ == '__main__':
		app.run(debug=True)

设置 session 过期时间

from datetime import timedelta
from flask import session, app
session.permanent = True
app.permanent_session_lifetime = timedelta(minutes=5)


## 使用 Cookie



	from flask import Flask, request, Response, make_response
	import time
	app = Flask(__name__)

	@app.route('/')
	def hello_world():
		return 'hello world'

	@app.route('/add')
	def login():
		res = Response('add cookies')
		res.set_cookie(key='name', value='letian', expires=time.time()+6*60)
		return res

	@app.route('/show')
	def show():
		return request.cookies.__str__()

	@app.route('/del')
	def del_cookie():
		res = Response('delete cookies')
		res.set_cookie('name', '', expires=0)
		# print res.headers
		# print res.data
		return res
	if __name__ == '__main__':
		app.run(host='0.0.0.0', debug=True)




## reference

- <http://www.letiantian.me/tags/flask/>














