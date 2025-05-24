---
layout: post
title:  "Deploying Python project on a linux without network access"
date:   2025-05-23 22:52:44 +0800
categories: jekyll update
---


## use `uv export` generate requirements.txt

  we suse `uv pip freeze > requirements.txt` to generate a requirements.txt

## get the glibc version of the target Linux server

`lld --version`

```text
 
 ldd (GNU libc) 2.34
 Copyright (C) 2021 Free Software Foundation, Inc.
 This is free software; see the source for copying conditions.  There is NO 
 warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. 
 Written by Roland McGrath and Ulrich Drepper.
 ```

## get the platform info

`python3 -m pip debug --verbose|less`

```text

pip version: pip 25.0.1 from /usr/local/lib/python3.13/site-packages/pip (python 3.13)
sys.version: 3.13.3 (main, May 23 2025, 20:28:13) [GCC 10.3.1]
pip version: pip 25.0.1 from /usr/local/lib/python3.13/site-packages/pip (python 3.13)
sys.version: 3.13.3 (main, May 23 2025, 20:28:13) [GCC 10.3.1]
sys.executable: /usr/local/bin/python3
sys.getdefaultencoding: utf-8
sys.getfilesystemencoding: utf-8
locale.getpreferredencoding: UTF-8
sys.platform: linux
sys.implementation:
  name: cpython
'cert' config value: Not specified
REQUESTS_CA_BUNDLE: None
CURL_CA_BUNDLE: None
pip._vendor.certifi.where(): /usr/local/lib/python3.13/site-packages/pip/_vendor/certifi/cacert.pem
pip._vendor.DEBUNDLED: False
vendored library versions:
  CacheControl==0.14.1
  distlib==0.3.9
  distro==1.9.0
  msgpack==1.1.0
  packaging==24.2
  platformdirs==4.3.6
  pyproject-hooks==1.2.0
  requests==2.32.3
  certifi==2024.08.30
  idna==3.10
  urllib3==1.26.20
  rich==13.9.4 (Unable to locate actual module version, using vendor.txt specified version)
  pygments==2.18.0
  typing_extensions==4.12.2 (Unable to locate actual module version, using vendor.txt specified >version)
  resolvelib==1.0.1
  setuptools==70.3.0 (Unable to locate actual module version, using vendor.txt specified version)
  tomli==2.2.1
  truststore==0.10.0
Compatible tags: 1002
  cp313-cp313-manylinux_2_34_x86_64
  cp313-cp313-manylinux_2_33_x86_64
```

## download wheel

`pip download -r ./requirements.txt -d ./linux_offlien_wheel --only-binary=:all: --platform manylinux2014_x86_64 --python-version 313 --abi cp313 --implementation cp`

**NOTICE:**

- Using "--platform=manylinux2014_x86_64" makes the package compatible with most Linux distributions.

## upload wheels to Linux

  Using FTP, you can conveniently use FTP to upload the wheels to the target Linux server.
  
## install package from local directory

`pip3 install --no-index --find-links file:///home/bcadmin/wheel/ -r requirement.txt`

