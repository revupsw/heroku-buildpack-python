Heroku buildpack: Python with Numpy, Scipy, scikit-learn
========================

This is a [Heroku buildpack](http://devcenter.heroku.com/articles/buildpacks) for Python apps, powered by [pip](http://www.pip-installer.org/).

This buildpack enables you to compile Numpy, Scipy, scikit-lern etc on Heroku.  

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
python-all-dev
python3-all-dev
liblapack-dev
libatlas-base-dev
cython
```

4.locate `.buildpacks` and `Aptfile` to your application root, then push to heroku.

**Caution:**
Scipy takes long time to compile. So Numpy, Scipy, scikit-learn at once will cause compile timeout on Heroku.  
To avoid it, exclude scikit-learn from your `requirements.txt` on first push, after that restore it and push again.


Usage
-----

Example usage:

    $ ls
    Procfile  requirements.txt  web.py

    $ heroku create --buildpack git://github.com/heroku/heroku-buildpack-python.git

    $ git push heroku master
    ...
    -----> Python app detected
    -----> Installing runtime (python-2.7.8)
    -----> Installing dependencies using pip
           Downloading/unpacking requests (from -r requirements.txt (line 1))
           Installing collected packages: requests
           Successfully installed requests
           Cleaning up...
    -----> Discovering process types
           Procfile declares types -> (none)

You can also add it to upcoming builds of an existing application:

    $ heroku config:add BUILDPACK_URL=git://github.com/heroku/heroku-buildpack-python.git

The buildpack will detect your app as Python if it has the file `requirements.txt` in the root.

It will use Pip to install your dependencies, vendoring a copy of the Python runtime into your slug.

Specify a Runtime
-----------------

You can also provide arbitrary releases Python with a `runtime.txt` file.

    $ cat runtime.txt
    python-3.4.2

Runtime options include:

- python-2.7.8
- python-3.4.2
- pypy-2.4.0 (unsupported, experimental)
- pypy3-2.3.1 (unsupported, experimental)

Other [unsupported runtimes](https://github.com/heroku/heroku-buildpack-python/tree/master/builds/runtimes) are available as well.
