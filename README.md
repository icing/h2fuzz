# h2fuzz - fuzzing mod_http2 in Apache httpd

This repository is a place of comfort and relaxation while you watch the fuzzing tests
pass (and stumble) against Apache httpd and its HTTP/2 implementation.

It is usable, but still work in progress. Anyone is invited to participate.

## Status

This repository, once configured, will build [honggfuzz](https://github.com/google/honggfuzz) from a master clone, `nghttp2` from the version listed in `configure.ac` and a checkout of the subversion `trunk` of Apache httpd.

The you may type 

```
make fuzz
```
and the components are built, installed into the prefix you configured and the tests are run.

## Install

There are some reprequisites your system needs before this all works smoothly, most of them documented on the [honggfuzz](https://github.com/google/honggfuzz) project itself, so in case of trouble, have a look there:
 * you need clang >= version 4.0 installed
 * you need to usual `automake` and `autoconf` things
 * you need to be able to clone from github (fair chance of that when you have this repository) and the subversion client (svn).

Then you run

```
> autoreconf -i
> ./configure --prefix=$PWD/gen/apache
> make
```
and it should build and install everything in `gen/apache` locally. (*Caveat*: the httpd config places its pid file in `/tmp` so that several honggfguzz setups can better work with each other.)

The configuration if httpd is minimal (for now), so there is no need to OpenSSL and other third party libs. A new c-lang is the major dependency.

## Usage

The following `make` commands are available:

```
make fuzz       #run honggfuzz endlessly on the installed httpd
make clean      #remove compilation results in subdirs
make install    #default target, checks out sources, builds and 
                #installs them in $prefix
make update     #performs git pull/svn update on honggfuzz and httpd copies
```

## ToDos

Some ideas around this fuzzing:

* h2fuzz uses some parts of the honggfuzz test files, but has copies of the httpd patch and httpd config file. Updates of these in honggfuzz are not effective in h2fuzz.
* `make reconfigure` would be nice to run all subdir `./configure` again and prep a new build&install
* top `./configure` should take arguments to influence CFLAGS. Other `-fsanitize=xxx` would be interesting
* `make update` in httpd should revert and re-apply patches
* would be nice of drop httpd patches just in a dir and have them applied for testing

## Credits

Many thanks to Robert Święcki who tested h2 with honggfuzz for some time and let me know when things get interesting. I decided that I have to run the tests myself also, to better save my users from hickups, so I felt the need to a comfortable setup.

Münster, 29.06.2017

Stefan Eissing