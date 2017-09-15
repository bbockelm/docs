OSG Software Stack -- Data Release -- 3.4.2-2 and 3.3.27-2
==========================================================

**Release Date**: 2017-08-14

Summary of changes
------------------

This release contains:

-   CA Certificates based on [IGTF 1.85](http://dist.eugridpma.info/distribution/igtf/current/CHANGES)
    -   Updated URL domain information for CyGrid (CY)

These [JIRA tickets](https://jira.opensciencegrid.org/issues/?jql=project%20%3D%20SOFTWARE%20AND%20fixVersion%20%3D%203.4.2-2%20ORDER%20BY%20priority%20DESC%2C%20key%20DESC) were addressed in this release.

Detailed changes are below. All of the documentation can be found in the [Release3](https://twiki.grid.iu.edu/bin/view/Documentation/Release3/) area of the TWiki.

Updating to the new release
---------------------------

### Update Repositories

To update to this series, you need [install the current OSG repositories](../../common/yum#install-osg-repositories).

### Update Software

Once the repositories are installed, you can update to this new release with:

``` console
[root@client ~] $ yum update
```

<span class="twiki-macro NOTE"></span> Please be aware that running `yum update` may also update other RPMs. You can exclude packages from being updated using the `--exclude=[package-name or glob]` option for the `yum` command.

<span class="twiki-macro NOTE"></span> Watch the yum update carefully for any messages about a `.rpmnew` file being created. That means that a configuration file had been editted, and a new default version was to be installed. In that case, RPM does not overwrite the editted configuration file but instead installs the new version with a `.rpmnew` extension. You will need to merge any edits that have made into the `.rpmnew` file and then move the merged version into place (that is, without the `.rpmnew` extension). Watch especially for `/etc/lcmaps.db`, which every site is expected to edit.

Need help?
----------

Do you need help with this release? [Contact us for help](../../common/help).

Detailed changes in this release
--------------------------------

### Packages

We added or updated the following packages to the production OSG yum repository. Note that in some cases, there are multiple RPMs for each package. You can click on any given package to see the set of RPMs or see the complete list below.

#### OSG 3.3

##### Enterprise Linux 6

-   [igtf-ca-certs-1.85-1.osg33.el6](https://koji.chtc.wisc.edu/koji/search?match=glob&type=build&terms=igtf-ca-certs-1.85-1.osg33.el6)
-   [osg-ca-certs-1.65-1.osg33.el6](https://koji.chtc.wisc.edu/koji/search?match=glob&type=build&terms=osg-ca-certs-1.65-1.osg33.el6)

##### Enterprise Linux 7

-   [igtf-ca-certs-1.85-1.osg33.el7](https://koji.chtc.wisc.edu/koji/search?match=glob&type=build&terms=igtf-ca-certs-1.85-1.osg33.el7)
-   [osg-ca-certs-1.65-1.osg33.el7](https://koji.chtc.wisc.edu/koji/search?match=glob&type=build&terms=osg-ca-certs-1.65-1.osg33.el7)

#### OSG 3.4

##### Enterprise Linux 6

-   [igtf-ca-certs-1.85-1.osg34.el6](https://koji.chtc.wisc.edu/koji/search?match=glob&type=build&terms=igtf-ca-certs-1.85-1.osg34.el6)
-   [osg-ca-certs-1.65-1.osg34.el6](https://koji.chtc.wisc.edu/koji/search?match=glob&type=build&terms=osg-ca-certs-1.65-1.osg34.el6)

##### Enterprise Linux 7

-   [igtf-ca-certs-1.85-1.osg34.el7](https://koji.chtc.wisc.edu/koji/search?match=glob&type=build&terms=igtf-ca-certs-1.85-1.osg34.el7)
-   [osg-ca-certs-1.65-1.osg34.el7](https://koji.chtc.wisc.edu/koji/search?match=glob&type=build&terms=osg-ca-certs-1.65-1.osg34.el7)

### RPMs

If you wish to manually update your system, you can run yum update against the following packages:

    igtf-ca-certs osg-ca-certs

If you wish to only update the RPMs that changed, the set of RPMs is:

#### OSG 3.3

##### Enterprise Linux 6

``` file
igtf-ca-certs-1.85-1.osg33.el6
osg-ca-certs-1.65-1.osg33.el6
```

##### Enterprise Linux 7

``` file
igtf-ca-certs-1.85-1.osg33.el7
osg-ca-certs-1.65-1.osg33.el7
```

#### OSG 3.4

##### Enterprise Linux 6

``` file
igtf-ca-certs-1.85-1.osg34.el6
osg-ca-certs-1.65-1.osg34.el6
```

##### Enterprise Linux 7

``` file
igtf-ca-certs-1.85-1.osg34.el7
osg-ca-certs-1.65-1.osg34.el7
```

