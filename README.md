# todo-app

![](presentation.gif)

## Technologies

* Frontend: Vue.js
* Backend: Django Rest Framework

## Introduction

This is a simple TODO list application, which allows user to add new tasks and delete completed tasks.<br>
This project is designed using API endpoints: 
* <b>getTasks(GET)</b>
* <b>addTask(POST)</b>
* <b>setStatus(PUT)</b>
* <b>resetStatus(PUT)</b>
* <b>delTask(DELETE)</b>

## Installation

#### Clone the project
```
$ git clone https://github.com/dlenskyi/todo-app.git
$ cd todo-app
```
#### Setting up Django
```
$ sudo pip3 install django
$ sudo pip3 install djangorestframework
$ sudo pip3 install django-cors-headers
```
#### Setting up Vue
```
$ sudo npm install -g vue-cli
$ sudo npm install axios
```
#### Setting up FontAwesome icons for Vue js
```
$ npm i --save @fortawesome/fontawesome-svg-core
$ npm i --save @fortawesome/free-solid-svg-icons
$ npm i --save @fortawesome/vue-fontawesome
$ npm i --save @fortawesome/free-brands-svg-icons
$ npm i --save @fortawesome/free-regular-svg-icons
```
## Usage
To make it work on your local server, run Django on local server:
```
$ sudo python3 manage.py runserver 127.0.0.1:8000
```
Open second terminal, enter the root directory, run Vue on local server:
```
$ cd frontend
$ npm run dev
```
Run in your browser `127.0.0.1:8080` to open the todo-app
## Enjoy :)
