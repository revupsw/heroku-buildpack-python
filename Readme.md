Heroku buildpack: Python with Numpy, Scipy, scikit-learn
========================

This is a [Heroku buildpack](http://devcenter.heroku.com/articles/buildpacks) for Python apps, powered by [pip](http://www.pip-installer.org/).

This buildpack enables you to compile Numpy, Scipy, scikit-lern etc on Heroku.  
! I recommends you to use [`conda-buildpack`](https://github.com/kennethreitz/conda-buildpack). Because of compile timeout issue (see below).  

Usage of this buildpack 
-----

1.use [heroku-buildpack-multi](https://github.com/heroku/heroku-buildpack-multi.git)  

```
$ heroku config:add BUILDPACK_URL=https://github.com/heroku/heroku-buildpack-multi.git
```

(You can set buildpack when creating the app. please see the [official document](https://devcenter.heroku.com/articles/buildpacks).)

2.edit `.buildpacks` for heroku-buildpack-multi as below.

```   
https://github.com/ddollar/heroku-buildpack-apt
https://github.com/icoxfog417/heroku-buildpack-python.git#statistics
```
    
3.edit `Aptfile` for [heroku-buildpack-apt](https://github.com/ddollar/heroku-buildpack-apt) as below.

```
build-essential
gfortran
libgfortran3
python-dev
libblas-dev
libatlas-base-dev
cython
```

4.locate `.buildpacks` and `Aptfile` to your application root, then push to heroku.

**Caution:**
Scipy takes long time to compile. So Numpy, Scipy, scikit-learn at once will cause compile timeout on Heroku.  
To avoid it, exclude scikit-learn from your `requirements.txt` on first push, after that restore it and push again.


# Heroku Buildpack: Python
![python](https://cloud.githubusercontent.com/assets/51578/13712821/b68a42ce-e793-11e5-96b0-d8eb978137ba.png)

This is the official [Heroku buildpack](https://devcenter.heroku.com/articles/buildpacks) for Python apps, powered by [pip](https://pip.pypa.io/) and other excellent software.

Recommended web frameworks include **Django** and **Flask**. The recommended webserver is **Gunicorn**. There are no restrictions around what software can be used (as long as it's pip-installable). Web processes must bind to `$PORT`, and only the HTTP protocol is permitted for incoming connections.

Some Python packages with obscure C dependencies (e.g. scipy) are [not compatible](https://devcenter.heroku.com/articles/python-c-deps). 

See it in Action
----------------

Deploying a Python application couldn't be easier:

    $ ls
    Procfile  requirements.txt  web.py

    $ heroku create --buildpack heroku/python

    $ git push heroku master
    ...
    -----> Python app detected
    -----> Installing python-2.7.11
         $ pip install -r requirements.txt
           Collecting requests (from -r requirements.txt (line 1))
             Downloading requests-2.9.1-py2.py3-none-any.whl (501kB)
           Installing collected packages: requests
           Successfully installed requests-2.9.1
           
    -----> Discovering process types
           Procfile declares types -> (none)

A `requirements.txt` file must be present at the root of your application's repository.

You can also specify the latest production release of this buildpack for upcoming builds of an existing application:

    $ heroku buildpacks:set heroku/python


Specify a Python Runtime
------------------------

Specific versions of the Python runtime can be specified with a `runtime.txt` file:

    $ cat runtime.txt
    python-3.5.1

Runtime options include:

- `python-2.7.11`
- `python-3.5.1`
- `pypy-5.0.1` (unsupported, experimental)

Other [unsupported runtimes](https://github.com/heroku/heroku-buildpack-python/tree/master/builds/runtimes) are available as well. Use at your own risk. 
