YUM Repositories
====================

About This Document
-------------------

This document introduces YUM repositories and how OSG uses them.
If you are unfamiliar with YUM, see the [documentation on using YUM and RPM](/release/yum-basics).

Repositories
------------

OSG hosts four public-facing repositories at [repo.opensciencegrid.org](https://repo.opensciencegrid.org/):

-   **release**: RPMs considered production-ready.
-   **testing**: RPMs not yet ready for release; expect bugs.
-   **development**: RPMs that are bleeding-edge;
      do not use without instruction from OSG Software and Release team members.
-   **contrib**: RPMs contributed from outside the OSG S&R team;
      no official OSG support.
-   **upcoming release/testing/development**: Similar to release/testing/development but for new versions of software that may require manual action after an update.

OSG's RPM packages rely also on external packages provided by supported OSes and EPEL. You must have the following repositories available and enabled:

-   OS repositories (SL 6/7, CentOS 6/7, or RHEL 6/7 repositories)
-   EPEL repositories
-   OSG repositories

If any of these repositories are missing, you may end up with missing dependencies.

!!! warning
    Other repositories, such as `jpackage`, `dag`, or `rpmforge`, are not supported and you may encounter problems if you use them.


Installing and Configuring Repositories
---------------------------------------

### Install the YUM priorities plugin

We use YUM priorities to tell YUM to prefer OSG packages over EPEL or OS packages.
It is important to install and enable the YUM priorities plugin before installing grid software to avoid getting the wrong versions.

1.  Install the YUM priorities package:

        :::console
        root@host # yum install yum-plugin-priorities

1.  Ensure that `/etc/yum.conf` has the following line in the `[main]` section:

        :::file
        plugins=1

### Install the EPEL repositories

OSG software depends on packages distributed via the [EPEL](http://fedoraproject.org/wiki/EPEL) repositories.
You must install and enable those first.

-   Install the EPEL repository, if not already present.  Choose the right version to match your OS version.

        :::console
        ## EPEL 6 (For RHEL 6, CentOS 6, and SL 6)
        root@host # rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-6.noarch.rpm
        ## EPEL 7 (For RHEL 7, CentOS 7, and SL 7)
        root@host # rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm

-   Verify that `/etc/yum.repos.d/epel.repo` exists; the `[epel]` section should contain:

    -   The line `enabled=1`

    -   Either no `priority` setting, or a `priority` setting that is 99 or higher

!!! warning
    If you have your own mirror or configuration of the EPEL repository, you **MUST** verify that the priority of the EPEL repository is either missing, or 99 or a higher number.
    The OSG repositories must have a better (numerically lower) priority than the EPEL repositories;
    you might have dependency resolution ("depsolving") issues otherwise.

### Automatic Updates

For production services, we suggest only changing software versions during controlled downtime.
Therefore we recommend security-only automatic updates or disabling automatic updates entirely.


To enable only security related automatic updates:

-   On Enterprise Linux 6, edit `/etc/sysconfig/yum-autoupdate` and set `USE_YUMSEC="true"`

-   On Enterprise Linux 7, edit `/etc/yum/yum-cron.conf` and set `update_cmd = security`


To disable automatic updates entirely:

-   On Enterprise Linux 6, edit `/etc/sysconfig/yum-autoupdate` and set `ENABLED="false"`

-   On Enterprise Linux 7, run:

        :::console
        root@host # service yum-cron stop

### Install the OSG Repositories

This document assumes a fresh install.
For instructions on upgrading from one OSG series to another, see the [release series document](/release/release_series#updating-from-old).

Install the OSG repositories:

    :::console
    root@host # rpm -Uvh <URL>

Where `<URL>` is one of the following:

| Series      |                  EL6 URL (for RHEL 6, CentOS 6, or SL 6)                  |                  EL7 URL (for RHEL 7, CentOS 7, or SL 7)                  |
|:------------|:-------------------------------------------------------------------------:|:-------------------------------------------------------------------------:|
| **OSG 3.3** | `https://repo.opensciencegrid.org/osg/3.3/osg-3.3-el6-release-latest.rpm` | `https://repo.opensciencegrid.org/osg/3.3/osg-3.3-el7-release-latest.rpm` |
| **OSG 3.4** | `https://repo.opensciencegrid.org/osg/3.4/osg-3.4-el6-release-latest.rpm` | `https://repo.opensciencegrid.org/osg/3.4/osg-3.4-el7-release-latest.rpm` |

The only OSG repository enabled by default is the release one. If you want to enable another one, such as `osg-testing`, then edit its file (e.g. `/etc/yum.repos.d/osg-testing.repo`) and change the enabled option from 0 to 1:

``` file
[osg-testing]
name=OSG Software for Enterprise Linux 7 - Testing - $basearch
#baseurl=https://repo.opensciencegrid.org/osg/3.4/el7/testing/$basearch
mirrorlist=https://repo.opensciencegrid.org/mirror/osg/3.4/el7/testing/$basearch
failovermethod=priority
priority=98
enabled=%RED%1%ENDCOLOR%
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-OSG
```


Repository Mirrors
------------------

If you run a large site (>20 nodes), you should consider setting up a local mirror for the OSG repositories.
A local YUM mirror allows you to reduce the amount of external bandwidth used when updating or installing packages.

Add the following to a file in `/etc/cron.d`:

    :::file
    %RED%RANDOM%ENDCOLOR% * * * * root rsync -aH rsync://repo.opensciencegrid.org/osg/ /var/www/html/osg/

Or, to mirror only a single repository:

    :::file
    %RED%RANDOM%ENDCOLOR% * * * * root rsync -aH rsync://repo.opensciencegrid.org/osg/%RED%OSG_RELEASE%ENDCOLOR%/el6/development /var/www/html/osg/%RED%OSG_RELEASE%ENDCOLOR%/el6


Replace %RED%RANDOM%ENDCOLOR% with a number between 0 and 59.

Replace %RED%OSG\_RELEASE%ENDCOLOR% with the OSG release you want to use (e.g. '3.3', or '3.4').

On your worker node, you can replace the `baseurl` line of `/etc/yum.repos.d/osg.repo` with the appropriate URL for your mirror.

If you are interested in having your mirror be part of the OSG's default set of mirrors, [please file a support ticket](https://support.opensciencegrid.org/helpdesk/tickets/new).

Reference
---------

-   [Basic use of Yum](/release/yum-basics.md)

