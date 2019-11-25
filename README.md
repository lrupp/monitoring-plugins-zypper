# About

This plugin checks for software updates on systems that use package management systems based on the zypper command found in (open)SUSE.

It checks for security, recommended and optional patches and also for optional package updates.

You can define the status by patch category. Use a commata to list more than one category to a state. 
Possible values are:    recommended,optional,security and packages

If you like to know the names of available patches and packages, use the "-v" option.

## Usage
```
  check_zypper [-w <category>] [-c <category>] [-t <timeout>] [-v]
  check_zypper [-h | --help]
  check_zypper [-V | --version]
```

## Options
``` 
  -c, --critical
      A patch with this category result in critical status.
      Default: security

  -f, --releasefile
      Use the given file to get informations about the distribution.
      Default: /etc/os-release (fallback: /etc/SuSE-release)

  -h, --help
      Print detailed help screen

  -i, --ignore <file>
      Ignore patches/packages that are mentioned in <file>
      Place the file in /etc/nagios/ or /etc/monitoring-plugins/ 
      and/or adapt the apparmor profile before using this feature!
      Just list one patch/package per line - example:

      patch:libtiff-devel
      # comment
      package:libtiff3
      package:libtiff-devel
      # comment
      whitelist:aaa_base
      # comment
      local_package:mypackage

  -o, --ignore_outdated
      Don't warn if a repository is outdated.

  -p, --no_perfdata
      Print no perfdata

  -r, --refresh_repos
      Tries to refresh the repositories before checking for updates.
      Note: this maybe needs an entry in /etc/sudoers like:
            nagios ALL = NOPASSWD: /usr/bin/zypper ref
            (and additional lines for the '-s' Option) if no 
             check-zypp-wrapper is available.

  -s, --use_sudo
      Zypper needs root privileges on some distributions 
      known: 10.1, SLE10, SLE12 and beyond).
      You can enable the script to use /usr/bin/sudo to start zypper.
      But don't forget to enable nopasswd sudo for the user 
      starting check_zypper

      Via lines like the two below on in /etc/sudoers:
          nagios ALL = NOPASSWD: /usr/bin/zypper services, \ 
                       /usr/bin/zypper --xmlout --non-interactive list-updates --type package --type patch

  -t, --timeout
      Just in case of problems, let's not hang Nagios and define a timeout.
      Default value is: 120 seconds

  -u, --check-vendor
      Check if installed packages are not from a supported vendor.

  -l, --check-local
      Check for local packages that are not in any 
      repository. NOTE: zypper just searches for
      exact the same version-release in the repositories, 
      so if the repositories do not contain
      old versions of the packages, this check may always 
      produce a warning.
      This check does not work on SLE-10

  -v, --verbose_output
      Print more information (useful only with Nagios v3.x).

  -w, --warning
      A patch with this category result in warning status.
      Default: recommended,optional,unsupported,local_package


  -V, --version
      Print version information

  -d, --debug
      Print debug output to STDERR
```

 The lines below contain all entries for your sudoers  file, if needed:
```
    nagios ALL = NOPASSWD: /usr/sbin/zypp-refresh "",\ 
                           /usr/bin/zypper refresh,\ 
                           /usr/bin/zypper services,\ 
                           /usr/bin/zypper --xmlout --non-interactive list-updates --type package --type patch
```
## Bugs

Please use [Github's issue tracker](https://github.com/lrupp/monitoring-plugins-zypper/issues/) to submit patches or suggest improvements.

Please include version information with all correspondence (when possible, use output from the --version option of the plugin itself).

