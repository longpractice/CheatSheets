binary pkg(.deb):

`$ar x file.deb` extract; `$ar t file.deb`: list files.

Pkg contents: debian-binary(text showing ver), control.tar.gz(meta-info), data.tar.gz(exe, docu, etc)

---

pkg header: `$dpkg --info pkg.deb`, `$dpkg --status pkg`,   `$apt-cache show pkg`

Header fields: Architecture, Replaces, Depends(can be generic services), conflicts

Virtual packages: no data, only provide generic service

Generic Services eg: mail-transport-agent, www-browser, xwindow-manager.

---

dpkg confs:

/var/lib/dpkg/status: all headers

/var/lib/dpkg/info/ *.list: list of files installed by a package

/var/lib/dpkg/info/ *.prein, *.postrm: action scripts

/var/lib/dpkg/info/ *conffiles: conf files of pkgs

When reinstall or update pkg, conf files are updated, if the old ones are modified by user, dpkg asks to use new or old. To force asking(can be useful if conf file deleted), use
```sh
$dpkg ... --force-confask
```
---
`$dpkg -i pkg.deb` install, compared to apt install, this fails if dependency not met.

`$dpkg -r pkg` or `$dpkg --purge pkg` difference is whether conf, usr data, etc are kept

---
`$dpkg --listfiles pkg`: list installed files

`$dpkg --status pkg`: show header

`$dpkg --search filename`: search file in installed files

`$dpkg --list pkg`: list known pkg

`$dpkg --contents pkg.deb`: list to be installed files

`$dpkg --info pkg.deb`: show pkg deb header

---
dpkg log location: /var/log/dpkg.log