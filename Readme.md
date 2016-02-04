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


# Heroku buildpack: Python
![python-banner](https://cloud.githubusercontent.com/assets/51578/8914205/ecf2047c-346b-11e5-98c5-42547f9f4410.jpg)

This is a [Heroku buildpack](http://devcenter.heroku.com/articles/buildpacks) for Python apps, powered by [pip](http://www.pip-installer.org/).

This buildpack supports running Django and Flask apps.


Usage
-----

Example usage:

    $ ls
    Procfile  requirements.txt  web.py

    $ heroku create --buildpack git://github.com/heroku/heroku-buildpack-python.git

    $ git push heroku master
    ...
    -----> Python app detected
    -----> Installing runtime (python-2.7.11)
    -----> Installing dependencies using pip
           Downloading/unpacking requests (from -r requirements.txt (line 1))
           Installing collected packages: requests
           Successfully installed requests
           Cleaning up...
    -----> Discovering process types
           Procfile declares types -> (none)

You can also add it to upcoming builds of an existing application:

    $ heroku buildpacks:set git://github.com/heroku/heroku-buildpack-python.git

The buildpack will detect your app as Python if it has the file `requirements.txt` in the root.

It will use Pip to install your dependencies, vendoring a copy of the Python runtime into your slug.

Specify a Runtime
-----------------

You can also provide arbitrary releases Python with a `runtime.txt` file.

    $ cat runtime.txt
    python-3.5.0

Runtime options include:

- python-2.7.11
- python-3.5.1
- pypy-4.0.1 (unsupported, experimental)
- pypy3-2.4.0 (unsupported, experimental)

Other [unsupported runtimes](https://github.com/heroku/heroku-buildpack-python/tree/master/builds/runtimes) are available as well.
