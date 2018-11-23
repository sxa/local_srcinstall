# local_srcinstall
ansible role for downloading and installing from source tarballs on UNIX platforms

Sample usage from within a playbook (short version)
```
    - role: local_srcinstall
      src_tarball: https://www.samba.org/ftp/ccache/ccache-3.4.2.tar.gz 
      installed_target: /usr/local/bin/ccache
```

Sample usage from within a playbook (long version)
```
    - role: local_srcinstall
      src_tarball: https://www.samba.org/ftp/ccache/ccache-3.4.2.tar.gz 
      installed_target: /usr/local/ccache/bin/ccache
      srcinstall_configure_params: --prefix=/usr/local/ccache
      srcinstall_version_required: 3.4.2
```
The first version is fairly straightforward. It will check for the presence
of /usr/local/bin/ccache and if it's not there then the source will be
downloaded and then `./configure --prefix=/usr/local && make && make install`
will be performed.

The long version causes various different things to happen.
- Firstly we are choosing to install into /usr/local/ccache as a prefix instead of /usr/local, so `srcinstall_configure_params` is added to the configure line
- Secondly a minimum version is specifed. In this case, a check will be run by parsing the last "word" of the first line obtained from running `--version` against the `installed_target`
- If `installed_target` does not exist, then a --version check will be done against just running the basename of the `installed_target` i.e. `ccache --version`
- The version will be checked against `srcinstall_version_required` and it will only be downloaded and built if it is backlevel

Gotchas:

- If you don't specify `srcinstall_version_required` then there is no check to see if the product is already on your machine, it ONLY checks the absolute path from `installed_target`
  This is fine for things that aren't likely to be installed via your package manager elsewhere, but may cause confusion otherwise
