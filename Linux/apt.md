## apt
Source list: /etc/apt/sources.list and /etc/apt/sources.list.d/filename.list

Package lists: 
- /var/lib/apt/lists/...InRelease (checksums and signatures)
- /var/lib/apt/lists/...Packages(headers of bin packages)
- /var/lib/apt/lists/...Sources(headers of src packages)

Downloaded packages: /var/cache/apt/archives/
* `$apt clean` purges this dir, `$apt autoclean` only clean pkgs removed from mirror

Update package lists: `$apt update` (verifies signature)

Install pkg: `$apt install`;

Remove pkg(keep usr data, log, conf, etc...): `$apt remove`

Purge pkg(remove also usr data, log, conf, etc...): `$apt purge`

Upgrade(only ones requiring no remove or install of other pkgs): `$apt upgrade`

Upgrade(permit remove and install of other pkgs): `$apt dist-upgrade`

Automatic/Manual pkg: installed only for dependency/manually 

`$apt manual pkg` `$apt auto pkg` mark as manual/automatic

`$aptitude why`: reason that pkg is installed(eg: manual/automatic)

`$apt-cache search pkg` search for pkg in local package lists

---
Trusted keys are pub keys used to verify signature: /etc/apt/trusted.gpg.d

/var/lib/apt/lists/...InRelease is to be verified by trusted keys.

`$apt-key fingerprint`: show all the keys

`$apt-key add`: add new keys
