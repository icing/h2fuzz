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
and it should build and install everything in `gen/apache` locally.

The configuration if httpd is minimal (for now), so there is no need to OpenSSL and other third party libs. A new c-lang is the major dependency.

## Credits

Many thanks to Robert Święcki who tested h2 with honggfuzz for some time and let me know when things get interesting. I decided that I have to run the tests myself also, to better save my users from hickups, so I felt the need to a comfortable setup.

Münster, 29.06.2017

Stefan Eissing