# centos7-glibc-python
Easy compile  Python 3.12 with latest GLIBC

Install GLIBC into /opt/glibc-2.39

wget http://ftp.gnu.org/gnu/glibc/glibc-2.39.tar.gz

tar zxvf glibc-2.39.tar.gz

cd glibc-2.39

mkdir build

cd build

LD_LIBRARY_PATH=""; ../configure --prefix=/opt/glibc-2.39

make -j4

LD_LIBRARY_PATH=""; make install


Altinstall Python 3.12.4 with latest openssl (install latest OpenSSL into /usr/local/openssl/ if required)

wget https://www.python.org/ftp/python/3.12.4/Python-3.12.4.tgz

tar zxvf Python-3.12.4.tgz

cd Python-3.12.4.tgz

scl enable devtoolset-11 bash

cd /usr/local/src/Python-3.12.4

OPENSSL_LIBS=/usr/local/openssl/lib64/libssl.so; ./configure --enable-optimizations --enable-loadable-sqlite-extensions --with-openssl=/usr/local/openssl --with-ssl-default-suites=openssl --with-openssl-rpath=/usr/local/openssl/lib64 CFLAGS="-I/usr/local/openssl/include/openssl " 
LDFLAGS="-L/usr/local/openssl/lib64  -Wl,-dynamic-linker=/opt/glibc-2.39/lib/ld-linux-x86-64.so.2 -Wl,-rpath=/opt/glibc-2.39/lib"

make -j4

make altinstall

update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.6 1

update-alternatives --install /usr/bin/python3 python3 /usr/local/bin/python3.12 2

update-alternatives --list

update-alternatives --config python3


You can verify now how Python version 3.12 works in comparison with default Python version 3.6:

python3.12 -c 'import sklearn;import torch;' =>> works OK without errors !!!

python3.6 -c 'import sklearn;import torch;' =>> crash with errors:

Traceback (most recent call last):
  File "<string>", line 1, in <module>
  File "/usr/local/lib64/python3.6/site-packages/torch/__init__.py", line 196, in <module>
    _load_global_deps()
  File "/usr/local/lib64/python3.6/site-packages/torch/__init__.py", line 149, in _load_global_deps
    ctypes.CDLL(lib_path, mode=ctypes.RTLD_GLOBAL)
  File "/usr/lib64/python3.6/ctypes/__init__.py", line 343, in __init__
    self._handle = _dlopen(self._name, mode)
OSError: dlopen: cannot load any more object with static TLS







