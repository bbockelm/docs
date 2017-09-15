OSG Software Release 3.3.0
==========================

**Release Date**: 2015-08-11

Summary of changes
------------------

This release contains:

-   Enterprise Linux 7: Worker node support only.
-    [XRootD 4.2.2](https://github.com/xrootd/xrootd/blob/v4.2.2/docs/ReleaseNotes.txt)
-   2 patches to HTCondor to better support HTCondor CE
    -   [Improve responsiveness of HTCondor CE by using light-weight polling](https://htcondor-wiki.cs.wisc.edu/index.cgi/tktview?tn=5190)
    -   [Prevent causing jobs to error out upon restart of JobRouter](https://htcondor-wiki.cs.wisc.edu/index.cgi/tktview?tn=5181)
-   improved logging of failures in lcmaps-plugins-verify-proxy
-   better credential handling and fix for crashes in lcamps-plugins-scas-client
-   CA Certificates are no longer repackaged for compatibility with old software
-   [VO Package v61](https://twiki.grid.iu.edu/bin/view/Operations/PackageV61)

These [JIRA tickets](https://jira.opensciencegrid.org/issues/?jql=project%20%3D%20SOFTWARE%20AND%20fixVersion%20%3D%203.3.0%20ORDER%20BY%20priority%20DESC) were addressed in this release.

Detailed changes are below. All of the documentation can be found in the [Release3](https://twiki.grid.iu.edu/bin/view/Documentation/Release3/) area of the TWiki.

Known Issues
------------

-   HTCondor CE does not work with HTCondor 8.3.6 (in upcoming) on EL 5 platforms. So, we are holding HTCondor at version 8.3.4 (in upcoming) for EL 5 platforms.
-   StashCache packages need to be manually configured
    -   Manual configuration for origin server
        -   Assuming that the origin server connects only to a redirector (not directly to cache server), minimal xrootd configuration is required. The configuration file, /etc/xrootd/xrootd-stashcache-origin-server.cfg, in this release is overkill. Here are recommended settings to use:

                :::file
                xrd.port 1094
                all.role server
                all.manager stash-redirector.example.com 1213
                all.export / nostage
                xrootd.trace emsg login stall redirect
                ofs.trace none
                xrd.trace conn
                cms.trace all
                sec.protocol  host
                sec.protbind  * none
                all.adminpath /var/run/xrootd
                all.pidpath /var/run/xrootd

-   Manual configuration for cache server
    -   In contrast to the origin server configuration, one needs to declare `pss.origin <stash-redirector.example.com>` instead of configuring the cmsd or manager (only the xrootd daemon is required on the cache server). More detailed configuration of cache server for StashCache is [here](https://confluence.grid.iu.edu/pages/viewpage.action?title=Installing+an+XRootD+server+for+Stash+Cache&spaceKey=STAS).
-   In both cases, administrator needs to set the path of custom configuration file for its xrootd/cmds instance in /etc/sysconfig/xrootd, For example, change the cmds default from:

        :::file
        CMSD_DEFAULT_OPTIONS="-l /var/log/xrootd/cmsd.log -c /etc/xrootd/xrootd-clustered.cfg -k fifo"
<br/>

    to


        :::file
        CMSD_DEFAULT_OPTIONS="-l /var/log/xrootd/cmsd.log -c /etc/xrootd/xrootd-stashcache-origin-server.marian -k fifo" 


Updating to the new release
---------------------------

### Update Repositories

To update to this series, you need [install the current OSG repositories](../../common/yum#install-osg-repositories).

### Update Software

Once the new repositories are installed, you can update to this new release with:

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

#### Enterprise Linux 6

-   [bestman2-2.3.0-25.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=bestman2-2.3.0-25.osg33.el6)
-   [bigtop-jsvc-0.3.0-1.1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=bigtop-jsvc-0.3.0-1.1.osg33.el6)
-   [bigtop-utils-0.4+300-1.cdh4.0.1.p0.3.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=bigtop-utils-0.4+300-1.cdh4.0.1.p0.3.osg33.el6)
-   [blahp-1.18.13.bosco-3.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=blahp-1.18.13.bosco-3.osg33.el6)
-   [bwctl-1.4-7.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=bwctl-1.4-7.osg33.el6)
-   [cctools-4.4.0-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=cctools-4.4.0-1.osg33.el6)
-   [cilogon-openid-ca-cert-1.1-2.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=cilogon-openid-ca-cert-1.1-2.osg33.el6)
-   [cilogon-osg-ca-cert-1.0-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=cilogon-osg-ca-cert-1.0-1.osg33.el6)
-   [cog-jglobus-axis-1.8.0-7.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=cog-jglobus-axis-1.8.0-7.osg33.el6)
-   [condor-8.3.6-1.4.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=condor-8.3.6-1.4.osg33.el6)
-   [condor-cron-1.0.9-4.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=condor-cron-1.0.9-4.osg33.el6)
-   [cvmfs-2.1.20-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=cvmfs-2.1.20-1.osg33.el6)
-   [cvmfs-config-osg-1.1-7.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=cvmfs-config-osg-1.1-7.osg33.el6)
-   [edg-mkgridmap-4.0.2-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=edg-mkgridmap-4.0.2-1.osg33.el6)
-   [emi-trustmanager-3.0.3-8.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=emi-trustmanager-3.0.3-8.osg33.el6)
-   [emi-trustmanager-axis-1.0.1-1.4.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=emi-trustmanager-axis-1.0.1-1.4.osg33.el6)
-   [emi-trustmanager-tomcat-3.0.0-10.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=emi-trustmanager-tomcat-3.0.0-10.osg33.el6)
-   [frontier-squid-2.7.STABLE9-24.1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=frontier-squid-2.7.STABLE9-24.1.osg33.el6)
-   [gfal2-plugin-xrootd-0.3.pre1-2.1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=gfal2-plugin-xrootd-0.3.pre1-2.1.osg33.el6)
-   [gip-1.3.11-6.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=gip-1.3.11-6.osg33.el6)
-   [glexec-0.9.11-1.1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=glexec-0.9.11-1.1.osg33.el6)
-   [glexec-wrapper-scripts-0.0.7-1.1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=glexec-wrapper-scripts-0.0.7-1.1.osg33.el6)
-   [glideinwms-3.2.10-1.1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=glideinwms-3.2.10-1.1.osg33.el6)
-   [glite-build-common-cpp-3.3.0.2-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=glite-build-common-cpp-3.3.0.2-1.osg33.el6)
-   [glite-ce-cream-client-api-c-1.14.0-4.10.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=glite-ce-cream-client-api-c-1.14.0-4.10.osg33.el6)
-   [glite-ce-cream-utils-1.2.0-4.3.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=glite-ce-cream-utils-1.2.0-4.3.osg33.el6)
-   [glite-lbjp-common-gsoap-plugin-3.1.2-2.2.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=glite-lbjp-common-gsoap-plugin-3.1.2-2.2.osg33.el6)
-   [glite-lbjp-common-gss-3.1.3-2.2.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=glite-lbjp-common-gss-3.1.3-2.2.osg33.el6)
-   [globus-authz-3.10-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-authz-3.10-1.osg33.el6)
-   [globus-authz-callout-error-3.5-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-authz-callout-error-3.5-1.osg33.el6)
-   [globus-callout-3.13-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-callout-3.13-1.osg33.el6)
-   [globus-common-15.27-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-common-15.27-1.osg33.el6)
-   [globus-ftp-client-8.19-1.2.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-ftp-client-8.19-1.2.osg33.el6)
-   [globus-ftp-control-6.6-1.1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-ftp-control-6.6-1.1.osg33.el6)
-   [globus-gass-cache-9.5-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-gass-cache-9.5-1.osg33.el6)
-   [globus-gass-cache-program-6.5-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-gass-cache-program-6.5-1.osg33.el6)
-   [globus-gass-copy-9.13-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-gass-copy-9.13-1.osg33.el6)
-   [globus-gass-server-ez-5.7-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-gass-server-ez-5.7-1.osg33.el6)
-   [globus-gass-transfer-8.8-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-gass-transfer-8.8-1.osg33.el6)
-   [globus-gatekeeper-10.9-1.2.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-gatekeeper-10.9-1.2.osg33.el6)
-   [globus-gfork-4.7-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-gfork-4.7-1.osg33.el6)
-   [globus-gram-audit-4.4-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-gram-audit-4.4-1.osg33.el6)
-   [globus-gram-client-13.12-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-gram-client-13.12-1.osg33.el6)
-   [globus-gram-client-tools-11.7-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-gram-client-tools-11.7-1.osg33.el6)
-   [globus-gram-job-manager-14.25-1.2.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-gram-job-manager-14.25-1.2.osg33.el6)
-   [globus-gram-job-manager-callout-error-3.5-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-gram-job-manager-callout-error-3.5-1.osg33.el6)
-   [globus-gram-job-manager-condor-2.5-1.1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-gram-job-manager-condor-2.5-1.1.osg33.el6)
-   [globus-gram-job-manager-fork-2.4-1.1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-gram-job-manager-fork-2.4-1.1.osg33.el6)
-   [globus-gram-job-manager-lsf-2.6-1.2.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-gram-job-manager-lsf-2.6-1.2.osg33.el6)
-   [globus-gram-job-manager-managedfork-0.2-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-gram-job-manager-managedfork-0.2-1.osg33.el6)
-   [globus-gram-job-manager-pbs-2.4-2.1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-gram-job-manager-pbs-2.4-2.1.osg33.el6)
-   [globus-gram-job-manager-scripts-6.7-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-gram-job-manager-scripts-6.7-1.osg33.el6)
-   [globus-gram-job-manager-sge-2.5-1.1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-gram-job-manager-sge-2.5-1.1.osg33.el6)
-   [globus-gram-protocol-12.12-2.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-gram-protocol-12.12-2.osg33.el6)
-   [globus-gridftp-server-7.20-1.1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-gridftp-server-7.20-1.1.osg33.el6)
-   [globus-gridftp-server-control-3.6-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-gridftp-server-control-3.6-1.osg33.el6)
-   [globus-gridmap-callout-error-2.4-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-gridmap-callout-error-2.4-1.osg33.el6)
-   [globus-gsi-callback-5.7-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-gsi-callback-5.7-1.osg33.el6)
-   [globus-gsi-cert-utils-9.10-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-gsi-cert-utils-9.10-1.osg33.el6)
-   [globus-gsi-credential-7.7-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-gsi-credential-7.7-1.osg33.el6)
-   [globus-gsi-openssl-error-3.5-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-gsi-openssl-error-3.5-1.osg33.el6)
-   [globus-gsi-proxy-core-7.7-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-gsi-proxy-core-7.7-1.osg33.el6)
-   [globus-gsi-proxy-ssl-5.7-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-gsi-proxy-ssl-5.7-1.osg33.el6)
-   [globus-gsi-sysconfig-6.8-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-gsi-sysconfig-6.8-1.osg33.el6)
-   [globus-gssapi-error-5.4-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-gssapi-error-5.4-1.osg33.el6)
-   [globus-gssapi-gsi-11.18-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-gssapi-gsi-11.18-1.osg33.el6)
-   [globus-gss-assist-10.13-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-gss-assist-10.13-1.osg33.el6)
-   [globus-io-11.4-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-io-11.4-1.osg33.el6)
-   [globus-openssl-module-4.6-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-openssl-module-4.6-1.osg33.el6)
-   [globus-proxy-utils-6.9-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-proxy-utils-6.9-1.osg33.el6)
-   [globus-rsl-10.9-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-rsl-10.9-1.osg33.el6)
-   [globus-scheduler-event-generator-5.10-2.1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-scheduler-event-generator-5.10-2.1.osg33.el6)
-   [globus-simple-ca-4.18-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-simple-ca-4.18-1.osg33.el6)
-   [globus-usage-4.4-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-usage-4.4-1.osg33.el6)
-   [globus-xio-5.7-1.1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-xio-5.7-1.1.osg33.el6)
-   [globus-xio-gsi-driver-3.7-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-xio-gsi-driver-3.7-1.osg33.el6)
-   [globus-xioperf-4.4-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-xioperf-4.4-1.osg33.el6)
-   [globus-xio-pipe-driver-3.7-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-xio-pipe-driver-3.7-1.osg33.el6)
-   [globus-xio-popen-driver-3.5-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-xio-popen-driver-3.5-1.osg33.el6)
-   [globus-xio-udt-driver-1.16-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-xio-udt-driver-1.16-1.osg33.el6)
-   [gratia-1.16.2-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=gratia-1.16.2-1.osg33.el6)
-   [gratia-probe-1.14.2-6.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=gratia-probe-1.14.2-6.osg33.el6)
-   [gratia-reporting-email-1.15.1-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=gratia-reporting-email-1.15.1-1.osg33.el6)
-   [gridftp-hdfs-0.5.4-19.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=gridftp-hdfs-0.5.4-19.osg33.el6)
-   [gsi-openssh-5.7-1.1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=gsi-openssh-5.7-1.1.osg33.el6)
-   [gums-1.4.4-3.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=gums-1.4.4-3.osg33.el6)
-   [hadoop-2.0.0+545-1.cdh4.1.1.p0.20.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=hadoop-2.0.0+545-1.cdh4.1.1.p0.20.osg33.el6)
-   [htcondor-ce-1.14-4.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=htcondor-ce-1.14-4.osg33.el6)
-   [I2util-1.1-2.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=I2util-1.1-2.osg33.el6)
-   [igtf-ca-certs-1.65-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=igtf-ca-certs-1.65-1.osg33.el6)
-   [javascriptrrd-1.1.1-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=javascriptrrd-1.1.1-1.osg33.el6)
-   [jetty-8.1.4.v20120524-2.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=jetty-8.1.4.v20120524-2.osg33.el6)
-   [jglobus-2.0.6-4.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=jglobus-2.0.6-4.osg33.el6)
-   [joda-time-1.5.2-7.2.tzdata2008d.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=joda-time-1.5.2-7.2.tzdata2008d.osg33.el6)
-   [koji-1.6.0-10.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=koji-1.6.0-10.osg33.el6)
-   [lcas-lcmaps-gt4-interface-0.2.6-1.1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=lcas-lcmaps-gt4-interface-0.2.6-1.1.osg33.el6)
-   [lcmaps-1.6.6-1.1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=lcmaps-1.6.6-1.1.osg33.el6)
-   [lcmaps-plugins-basic-1.7.0-2.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=lcmaps-plugins-basic-1.7.0-2.osg33.el6)
-   [lcmaps-plugins-glexec-tracking-0.1.6-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=lcmaps-plugins-glexec-tracking-0.1.6-1.osg33.el6)
-   [lcmaps-plugins-gums-client-0.0.2-4.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=lcmaps-plugins-gums-client-0.0.2-4.osg33.el6)
-   [lcmaps-plugins-scas-client-0.5.5-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=lcmaps-plugins-scas-client-0.5.5-1.osg33.el6)
-   [lcmaps-plugins-verify-proxy-1.5.7-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=lcmaps-plugins-verify-proxy-1.5.7-1.osg33.el6)
-   [llrun-0.1.3-1.3.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=llrun-0.1.3-1.3.osg33.el6)
-   [mash-0.5.22-3.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=mash-0.5.22-3.osg33.el6)
-   [mkgltempdir-0.0.5-1.1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=mkgltempdir-0.0.5-1.1.osg33.el6)
-   [myproxy-6.1.12-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=myproxy-6.1.12-1.osg33.el6)
-   [ndt-3.6.5-3.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=ndt-3.6.5-3.osg33.el6)
-   [netlogger-4.2.0-9.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=netlogger-4.2.0-9.osg33.el6)
-   [nuttcp-6.1.2-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=nuttcp-6.1.2-1.osg33.el6)
-   [osg-build-1.6.0-2.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-build-1.6.0-2.osg33.el6)
-   [osg-ca-certs-1.47-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-ca-certs-1.47-1.osg33.el6)
-   [osg-ca-certs-updater-1.0-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-ca-certs-updater-1.0-1.osg33.el6)
-   [osg-ca-scripts-1.1.5-2.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-ca-scripts-1.1.5-2.osg33.el6)
-   [osg-ce-3.3-3.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-ce-3.3-3.osg33.el6)
-   [osg-cert-scripts-2.7.2-2.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-cert-scripts-2.7.2-2.osg33.el6)
-   [osg-cleanup-1.7.2-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-cleanup-1.7.2-1.osg33.el6)
-   [osg-configure-1.1.1-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-configure-1.1.1-1.osg33.el6)
-   [osg-control-1.0.1-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-control-1.0.1-1.osg33.el6)
-   [osg-gridftp-3.3-2.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-gridftp-3.3-2.osg33.el6)
-   [osg-gridftp-hdfs-3.3-2.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-gridftp-hdfs-3.3-2.osg33.el6)
-   [osg-gridftp-xrootd-3.3-2.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-gridftp-xrootd-3.3-2.osg33.el6)
-   [osg-gums-3.3-2.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-gums-3.3-2.osg33.el6)
-   [osg-info-services-1.0.2-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-info-services-1.0.2-1.osg33.el6)
-   [osg-java7-compat-1.0-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-java7-compat-1.0-1.osg33.el6)
-   [osg-oasis-5-2.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-oasis-5-2.osg33.el6)
-   [osg-pki-tools-1.2.12-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-pki-tools-1.2.12-1.osg33.el6)
-   [osg-release-3.3-2.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-release-3.3-2.osg33.el6)
-   [osg-release-itb-3.3-2.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-release-itb-3.3-2.osg33.el6)
-   [osg-se-bestman-3.3-2.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-se-bestman-3.3-2.osg33.el6)
-   [osg-se-bestman-xrootd-3.3-2.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-se-bestman-xrootd-3.3-2.osg33.el6)
-   [osg-se-hadoop-3.3-2.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-se-hadoop-3.3-2.osg33.el6)
-   [osg-system-profiler-1.2.0-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-system-profiler-1.2.0-1.osg33.el6)
-   [osg-test-1.4.26-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-test-1.4.26-1.osg33.el6)
-   [osg-tested-internal-3.3-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-tested-internal-3.3-1.osg33.el6)
-   [osg-version-3.3.0-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-version-3.3.0-1.osg33.el6)
-   [osg-vo-map-0.0.1-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-vo-map-0.0.1-1.osg33.el6)
-   [osg-voms-3.3-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-voms-3.3-1.osg33.el6)
-   [osg-webapp-common-1-2.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-webapp-common-1-2.osg33.el6)
-   [osg-wn-client-3.3-5.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-wn-client-3.3-5.osg33.el6)
-   [owamp-3.2rc4-2.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=owamp-3.2rc4-2.osg33.el6)
-   [pegasus-4.3.1-1.3.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=pegasus-4.3.1-1.3.osg33.el6)
-   [privilege-xacml-2.6.4-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=privilege-xacml-2.6.4-1.osg33.el6)
-   [rsv-3.10.2-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=rsv-3.10.2-1.osg33.el6)
-   [rsv-perfsonar-1.0.19-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=rsv-perfsonar-1.0.19-1.osg33.el6)
-   [rsv-vo-gwms-1.0.1-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=rsv-vo-gwms-1.0.1-1.osg33.el6)
-   [stashcache-0.3-4.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=stashcache-0.3-4.osg33.el6)
-   [stashcache-daemon-0.2-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=stashcache-daemon-0.2-1.osg33.el6)
-   [uberftp-2.8-2.1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=uberftp-2.8-2.1.osg33.el6)
-   [vo-client-61-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=vo-client-61-1.osg33.el6)
-   [voms-2.0.12-3.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=voms-2.0.12-3.osg33.el6)
-   [voms-admin-client-2.0.17-1.1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=voms-admin-client-2.0.17-1.1.osg33.el6)
-   [voms-admin-server-2.7.0-1.14.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=voms-admin-server-2.7.0-1.14.osg33.el6)
-   [voms-api-java-2.0.8-1.6.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=voms-api-java-2.0.8-1.6.osg33.el6)
-   [voms-mysql-plugin-3.1.6-1.1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=voms-mysql-plugin-3.1.6-1.1.osg33.el6)
-   [web100\_userland-1.7-6.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=web100_userland-1.7-6.osg33.el6)
-   [xacml-1.5.0-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=xacml-1.5.0-1.osg33.el6)
-   [xrootd-4.2.2-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=xrootd-4.2.2-1.osg33.el6)
-   [xrootd-dsi-3.0.4-16.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=xrootd-dsi-3.0.4-16.osg33.el6)
-   [xrootd-hdfs-1.8.4-4.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=xrootd-hdfs-1.8.4-4.osg33.el6)
-   [xrootd-lcmaps-0.0.7-11.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=xrootd-lcmaps-0.0.7-11.osg33.el6)
-   [xrootd-status-probe-0.0.3-11.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=xrootd-status-probe-0.0.3-11.osg33.el6)
-   [xrootd-voms-plugin-0.2.0-1.6.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=xrootd-voms-plugin-0.2.0-1.6.osg33.el6)
-   [zookeeper-3.4.3+15-1.cdh4.0.1.p0.3.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=zookeeper-3.4.3+15-1.cdh4.0.1.p0.3.osg33.el6)

#### Enterprise Linux 7

-   [bigtop-jsvc-0.3.0-1.1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=bigtop-jsvc-0.3.0-1.1.osg33.el7)
-   [bigtop-utils-0.4+300-1.cdh4.0.1.p0.3.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=bigtop-utils-0.4+300-1.cdh4.0.1.p0.3.osg33.el7)
-   [blahp-1.18.13.bosco-3.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=blahp-1.18.13.bosco-3.osg33.el7)
-   [bwctl-1.4-7.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=bwctl-1.4-7.osg33.el7)
-   [cctools-4.4.0-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=cctools-4.4.0-1.osg33.el7)
-   [cilogon-openid-ca-cert-1.1-2.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=cilogon-openid-ca-cert-1.1-2.osg33.el7)
-   [cilogon-osg-ca-cert-1.0-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=cilogon-osg-ca-cert-1.0-1.osg33.el7)
-   [cog-jglobus-axis-1.8.0-8.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=cog-jglobus-axis-1.8.0-8.osg33.el7)
-   [condor-8.3.6-1.4.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=condor-8.3.6-1.4.osg33.el7)
-   [condor-cron-1.0.9-4.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=condor-cron-1.0.9-4.osg33.el7)
-   [cvmfs-2.1.20-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=cvmfs-2.1.20-1.osg33.el7)
-   [cvmfs-config-osg-1.1-7.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=cvmfs-config-osg-1.1-7.osg33.el7)
-   [edg-mkgridmap-4.0.2-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=edg-mkgridmap-4.0.2-1.osg33.el7)
-   [emi-trustmanager-3.0.3-8.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=emi-trustmanager-3.0.3-8.osg33.el7)
-   [emi-trustmanager-axis-1.0.1-1.4.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=emi-trustmanager-axis-1.0.1-1.4.osg33.el7)
-   [frontier-squid-2.7.STABLE9-24.1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=frontier-squid-2.7.STABLE9-24.1.osg33.el7)
-   [gfal2-plugin-xrootd-0.3.pre1-2.1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=gfal2-plugin-xrootd-0.3.pre1-2.1.osg33.el7)
-   [gip-1.3.11-6.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=gip-1.3.11-6.osg33.el7)
-   [glexec-0.9.11-1.1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=glexec-0.9.11-1.1.osg33.el7)
-   [glexec-wrapper-scripts-0.0.7-1.1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=glexec-wrapper-scripts-0.0.7-1.1.osg33.el7)
-   [glideinwms-3.2.10-1.1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=glideinwms-3.2.10-1.1.osg33.el7)
-   [glite-build-common-cpp-3.3.0.2-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=glite-build-common-cpp-3.3.0.2-1.osg33.el7)
-   [glite-lbjp-common-gsoap-plugin-3.1.2-2.2.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=glite-lbjp-common-gsoap-plugin-3.1.2-2.2.osg33.el7)
-   [glite-lbjp-common-gss-3.1.3-2.2.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=glite-lbjp-common-gss-3.1.3-2.2.osg33.el7)
-   [globus-authz-3.10-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-authz-3.10-1.osg33.el7)
-   [globus-authz-callout-error-3.5-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-authz-callout-error-3.5-1.osg33.el7)
-   [globus-callout-3.13-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-callout-3.13-1.osg33.el7)
-   [globus-common-15.27-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-common-15.27-1.osg33.el7)
-   [globus-ftp-client-8.19-1.2.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-ftp-client-8.19-1.2.osg33.el7)
-   [globus-ftp-control-6.6-1.1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-ftp-control-6.6-1.1.osg33.el7)
-   [globus-gass-cache-9.5-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-gass-cache-9.5-1.osg33.el7)
-   [globus-gass-cache-program-6.5-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-gass-cache-program-6.5-1.osg33.el7)
-   [globus-gass-copy-9.13-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-gass-copy-9.13-1.osg33.el7)
-   [globus-gass-server-ez-5.7-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-gass-server-ez-5.7-1.osg33.el7)
-   [globus-gass-transfer-8.8-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-gass-transfer-8.8-1.osg33.el7)
-   [globus-gatekeeper-10.9-1.2.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-gatekeeper-10.9-1.2.osg33.el7)
-   [globus-gfork-4.7-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-gfork-4.7-1.osg33.el7)
-   [globus-gram-audit-4.4-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-gram-audit-4.4-1.osg33.el7)
-   [globus-gram-client-13.12-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-gram-client-13.12-1.osg33.el7)
-   [globus-gram-client-tools-11.7-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-gram-client-tools-11.7-1.osg33.el7)
-   [globus-gram-job-manager-14.25-1.2.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-gram-job-manager-14.25-1.2.osg33.el7)
-   [globus-gram-job-manager-callout-error-3.5-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-gram-job-manager-callout-error-3.5-1.osg33.el7)
-   [globus-gram-job-manager-condor-2.5-1.1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-gram-job-manager-condor-2.5-1.1.osg33.el7)
-   [globus-gram-job-manager-fork-2.4-1.1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-gram-job-manager-fork-2.4-1.1.osg33.el7)
-   [globus-gram-job-manager-lsf-2.6-1.2.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-gram-job-manager-lsf-2.6-1.2.osg33.el7)
-   [globus-gram-job-manager-managedfork-0.2-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-gram-job-manager-managedfork-0.2-1.osg33.el7)
-   [globus-gram-job-manager-pbs-2.4-2.1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-gram-job-manager-pbs-2.4-2.1.osg33.el7)
-   [globus-gram-job-manager-scripts-6.7-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-gram-job-manager-scripts-6.7-1.osg33.el7)
-   [globus-gram-job-manager-sge-2.5-1.1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-gram-job-manager-sge-2.5-1.1.osg33.el7)
-   [globus-gram-protocol-12.12-2.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-gram-protocol-12.12-2.osg33.el7)
-   [globus-gridftp-server-7.20-1.1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-gridftp-server-7.20-1.1.osg33.el7)
-   [globus-gridftp-server-control-3.6-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-gridftp-server-control-3.6-1.osg33.el7)
-   [globus-gridmap-callout-error-2.4-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-gridmap-callout-error-2.4-1.osg33.el7)
-   [globus-gsi-callback-5.7-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-gsi-callback-5.7-1.osg33.el7)
-   [globus-gsi-cert-utils-9.10-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-gsi-cert-utils-9.10-1.osg33.el7)
-   [globus-gsi-credential-7.7-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-gsi-credential-7.7-1.osg33.el7)
-   [globus-gsi-openssl-error-3.5-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-gsi-openssl-error-3.5-1.osg33.el7)
-   [globus-gsi-proxy-core-7.7-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-gsi-proxy-core-7.7-1.osg33.el7)
-   [globus-gsi-proxy-ssl-5.7-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-gsi-proxy-ssl-5.7-1.osg33.el7)
-   [globus-gsi-sysconfig-6.8-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-gsi-sysconfig-6.8-1.osg33.el7)
-   [globus-gssapi-error-5.4-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-gssapi-error-5.4-1.osg33.el7)
-   [globus-gssapi-gsi-11.18-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-gssapi-gsi-11.18-1.osg33.el7)
-   [globus-gss-assist-10.13-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-gss-assist-10.13-1.osg33.el7)
-   [globus-io-11.4-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-io-11.4-1.osg33.el7)
-   [globus-openssl-module-4.6-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-openssl-module-4.6-1.osg33.el7)
-   [globus-proxy-utils-6.9-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-proxy-utils-6.9-1.osg33.el7)
-   [globus-rsl-10.9-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-rsl-10.9-1.osg33.el7)
-   [globus-scheduler-event-generator-5.10-2.1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-scheduler-event-generator-5.10-2.1.osg33.el7)
-   [globus-simple-ca-4.18-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-simple-ca-4.18-1.osg33.el7)
-   [globus-usage-4.4-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-usage-4.4-1.osg33.el7)
-   [globus-xio-5.7-1.1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-xio-5.7-1.1.osg33.el7)
-   [globus-xio-gsi-driver-3.7-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-xio-gsi-driver-3.7-1.osg33.el7)
-   [globus-xioperf-4.4-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-xioperf-4.4-1.osg33.el7)
-   [globus-xio-pipe-driver-3.7-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-xio-pipe-driver-3.7-1.osg33.el7)
-   [globus-xio-popen-driver-3.5-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-xio-popen-driver-3.5-1.osg33.el7)
-   [globus-xio-udt-driver-1.16-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=globus-xio-udt-driver-1.16-1.osg33.el7)
-   [gratia-1.16.2-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=gratia-1.16.2-1.osg33.el7)
-   [gratia-probe-1.14.2-6.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=gratia-probe-1.14.2-6.osg33.el7)
-   [gratia-reporting-email-1.15.1-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=gratia-reporting-email-1.15.1-1.osg33.el7)
-   [gsi-openssh-5.7-1.1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=gsi-openssh-5.7-1.1.osg33.el7)
-   [htcondor-ce-1.14-4.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=htcondor-ce-1.14-4.osg33.el7)
-   [I2util-1.1-2.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=I2util-1.1-2.osg33.el7)
-   [igtf-ca-certs-1.65-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=igtf-ca-certs-1.65-1.osg33.el7)
-   [javascriptrrd-1.1.1-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=javascriptrrd-1.1.1-1.osg33.el7)
-   [jetty-8.1.4.v20120524-2.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=jetty-8.1.4.v20120524-2.osg33.el7)
-   [joda-time-1.5.2-7.2.tzdata2008d.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=joda-time-1.5.2-7.2.tzdata2008d.osg33.el7)
-   [koji-1.6.0-10.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=koji-1.6.0-10.osg33.el7)
-   [lcas-lcmaps-gt4-interface-0.2.6-1.1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=lcas-lcmaps-gt4-interface-0.2.6-1.1.osg33.el7)
-   [lcmaps-1.6.6-1.1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=lcmaps-1.6.6-1.1.osg33.el7)
-   [lcmaps-plugins-basic-1.7.0-2.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=lcmaps-plugins-basic-1.7.0-2.osg33.el7)
-   [lcmaps-plugins-glexec-tracking-0.1.6-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=lcmaps-plugins-glexec-tracking-0.1.6-1.osg33.el7)
-   [lcmaps-plugins-gums-client-0.0.2-4.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=lcmaps-plugins-gums-client-0.0.2-4.osg33.el7)
-   [lcmaps-plugins-scas-client-0.5.5-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=lcmaps-plugins-scas-client-0.5.5-1.osg33.el7)
-   [lcmaps-plugins-verify-proxy-1.5.7-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=lcmaps-plugins-verify-proxy-1.5.7-1.osg33.el7)
-   [llrun-0.1.3-1.3.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=llrun-0.1.3-1.3.osg33.el7)
-   [mash-0.5.22-3.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=mash-0.5.22-3.osg33.el7)
-   [mkgltempdir-0.0.5-1.1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=mkgltempdir-0.0.5-1.1.osg33.el7)
-   [myproxy-6.1.12-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=myproxy-6.1.12-1.osg33.el7)
-   [ndt-3.6.5-3.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=ndt-3.6.5-3.osg33.el7)
-   [netlogger-4.2.0-9.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=netlogger-4.2.0-9.osg33.el7)
-   [nuttcp-6.1.2-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=nuttcp-6.1.2-1.osg33.el7)
-   [osg-build-1.6.0-2.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-build-1.6.0-2.osg33.el7)
-   [osg-ca-certs-1.47-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-ca-certs-1.47-1.osg33.el7)
-   [osg-ca-certs-updater-1.0-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-ca-certs-updater-1.0-1.osg33.el7)
-   [osg-ca-scripts-1.1.5-2.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-ca-scripts-1.1.5-2.osg33.el7)
-   [osg-ce-3.3-3\_clipped.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-ce-3.3-3_clipped.osg33.el7)
-   [osg-cert-scripts-2.7.2-2.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-cert-scripts-2.7.2-2.osg33.el7)
-   [osg-cleanup-1.7.2-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-cleanup-1.7.2-1.osg33.el7)
-   [osg-configure-1.1.1-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-configure-1.1.1-1.osg33.el7)
-   [osg-control-1.0.1-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-control-1.0.1-1.osg33.el7)
-   [osg-gridftp-3.3-2\_clipped.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-gridftp-3.3-2_clipped.osg33.el7)
-   [osg-gridftp-hdfs-3.3-2.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-gridftp-hdfs-3.3-2.osg33.el7)
-   [osg-gridftp-xrootd-3.3-2.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-gridftp-xrootd-3.3-2.osg33.el7)
-   [osg-gums-3.3-2.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-gums-3.3-2.osg33.el7)
-   [osg-info-services-1.0.2-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-info-services-1.0.2-1.osg33.el7)
-   [osg-java7-compat-1.0-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-java7-compat-1.0-1.osg33.el7)
-   [osg-oasis-5-2.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-oasis-5-2.osg33.el7)
-   [osg-pki-tools-1.2.12-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-pki-tools-1.2.12-1.osg33.el7)
-   [osg-release-3.3-2.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-release-3.3-2.osg33.el7)
-   [osg-release-itb-3.3-2.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-release-itb-3.3-2.osg33.el7)
-   [osg-se-bestman-3.3-2.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-se-bestman-3.3-2.osg33.el7)
-   [osg-se-bestman-xrootd-3.3-2.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-se-bestman-xrootd-3.3-2.osg33.el7)
-   [osg-se-hadoop-3.3-2.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-se-hadoop-3.3-2.osg33.el7)
-   [osg-system-profiler-1.2.0-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-system-profiler-1.2.0-1.osg33.el7)
-   [osg-test-1.4.25-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-test-1.4.25-1.osg33.el7)
-   [osg-tested-internal-3.3-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-tested-internal-3.3-1.osg33.el7)
-   [osg-version-3.3.0-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-version-3.3.0-1.osg33.el7)
-   [osg-vo-map-0.0.1-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-vo-map-0.0.1-1.osg33.el7)
-   [osg-voms-3.3-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-voms-3.3-1.osg33.el7)
-   [osg-webapp-common-1-2.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-webapp-common-1-2.osg33.el7)
-   [osg-wn-client-3.3-5.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-wn-client-3.3-5.osg33.el7)
-   [owamp-3.2rc4-2.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=owamp-3.2rc4-2.osg33.el7)
-   [pegasus-4.3.1-1.3.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=pegasus-4.3.1-1.3.osg33.el7)
-   [rsv-3.10.2-1\_clipped.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=rsv-3.10.2-1_clipped.osg33.el7)
-   [rsv-perfsonar-1.0.19-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=rsv-perfsonar-1.0.19-1.osg33.el7)
-   [rsv-vo-gwms-1.0.1-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=rsv-vo-gwms-1.0.1-1.osg33.el7)
-   [stashcache-0.3-4.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=stashcache-0.3-4.osg33.el7)
-   [stashcache-daemon-0.2-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=stashcache-daemon-0.2-1.osg33.el7)
-   [uberftp-2.8-2.1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=uberftp-2.8-2.1.osg33.el7)
-   [vo-client-61-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=vo-client-61-1.osg33.el7)
-   [voms-2.0.12-3.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=voms-2.0.12-3.osg33.el7)
-   [voms-admin-client-2.0.17-1.1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=voms-admin-client-2.0.17-1.1.osg33.el7)
-   [voms-api-java-2.0.8-1.6.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=voms-api-java-2.0.8-1.6.osg33.el7)
-   [voms-mysql-plugin-3.1.6-1.1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=voms-mysql-plugin-3.1.6-1.1.osg33.el7)
-   [web100\_userland-1.7-6.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=web100_userland-1.7-6.osg33.el7)
-   [xacml-1.5.0-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=xacml-1.5.0-1.osg33.el7)
-   [xrootd-4.2.2-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=xrootd-4.2.2-1.osg33.el7)
-   [xrootd-dsi-3.0.4-16.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=xrootd-dsi-3.0.4-16.osg33.el7)
-   [xrootd-lcmaps-0.0.7-11.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=xrootd-lcmaps-0.0.7-11.osg33.el7)
-   [xrootd-status-probe-0.0.3-11.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=xrootd-status-probe-0.0.3-11.osg33.el7)
-   [xrootd-voms-plugin-0.2.0-1.6.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=xrootd-voms-plugin-0.2.0-1.6.osg33.el7)

### RPMs

If you wish to manually update your system, you can run yum update against the following packages:

    bestman2-client bestman2-client-libs bestman2-common-libs bestman2-server bestman2-server-dep-libs bestman2-server-libs bestman2-tester bestman2-tester-libs bigtop-jsvc bigtop-jsvc-debuginfo bigtop-utils blahp blahp-debuginfo bwctl bwctl-client bwctl-debuginfo bwctl-devel bwctl-server cctools-chirp cctools-debuginfo cctools-doc cctools-dttools cctools-makeflow cctools-parrot cctools-resource_monitor cctools-sand cctools-wavefront cctools-work_queue cilogon-openid-ca-cert cilogon-osg-ca-cert cog-jglobus-axis condor condor-all condor-bosco condor-classads condor-classads-devel condor-cream-gahp condor-cron condor-debuginfo condor-kbdd condor-procd condor-python condor-std-universe condor-test condor-vm-gahp cvmfs cvmfs-config-osg cvmfs-devel cvmfs-server cvmfs-unittests edg-mkgridmap emi-trustmanager emi-trustmanager-axis emi-trustmanager-tomcat frontier-squid frontier-squid-debuginfo gfal2-plugin-xrootd gfal2-plugin-xrootd-debuginfo gip glexec glexec-debuginfo glexec-wrapper-scripts glideinwms-factory glideinwms-factory-condor glideinwms-glidecondor-tools glideinwms-libs glideinwms-minimal-condor glideinwms-usercollector glideinwms-userschedd glideinwms-vofrontend glideinwms-vofrontend-standalone glite-build-common-cpp glite-ce-cream-client-api-c glite-ce-cream-client-api-c-debuginfo glite-ce-cream-client-devel glite-ce-cream-utils glite-lbjp-common-gsoap-plugin glite-lbjp-common-gsoap-plugin-debuginfo glite-lbjp-common-gsoap-plugin-devel glite-lbjp-common-gss glite-lbjp-common-gss-debuginfo glite-lbjp-common-gss-devel globus-authz globus-authz-callout-error globus-authz-callout-error-debuginfo globus-authz-callout-error-devel globus-authz-callout-error-doc globus-authz-debuginfo globus-authz-devel globus-authz-doc globus-callout globus-callout-debuginfo globus-callout-devel globus-callout-doc globus-common globus-common-debuginfo globus-common-devel globus-common-doc globus-common-progs globus-ftp-client globus-ftp-client-debuginfo globus-ftp-client-devel globus-ftp-client-doc globus-ftp-control globus-ftp-control-debuginfo globus-ftp-control-devel globus-ftp-control-doc globus-gass-cache globus-gass-cache-debuginfo globus-gass-cache-devel globus-gass-cache-doc globus-gass-cache-program globus-gass-cache-program-debuginfo globus-gass-copy globus-gass-copy-debuginfo globus-gass-copy-devel globus-gass-copy-doc globus-gass-copy-progs globus-gass-server-ez globus-gass-server-ez-debuginfo globus-gass-server-ez-devel globus-gass-server-ez-progs globus-gass-transfer globus-gass-transfer-debuginfo globus-gass-transfer-devel globus-gass-transfer-doc globus-gatekeeper globus-gatekeeper-debuginfo globus-gfork globus-gfork-debuginfo globus-gfork-devel globus-gfork-progs globus-gram-audit globus-gram-client globus-gram-client-debuginfo globus-gram-client-devel globus-gram-client-doc globus-gram-client-tools globus-gram-client-tools-debuginfo globus-gram-job-manager globus-gram-job-manager-callout-error globus-gram-job-manager-callout-error-debuginfo globus-gram-job-manager-callout-error-devel globus-gram-job-manager-callout-error-doc globus-gram-job-manager-condor globus-gram-job-manager-debuginfo globus-gram-job-manager-fork globus-gram-job-manager-fork-debuginfo globus-gram-job-manager-fork-setup-poll globus-gram-job-manager-fork-setup-seg globus-gram-job-manager-lsf globus-gram-job-manager-lsf-debuginfo globus-gram-job-manager-lsf-setup-poll globus-gram-job-manager-lsf-setup-seg globus-gram-job-manager-managedfork globus-gram-job-manager-pbs globus-gram-job-manager-pbs-debuginfo globus-gram-job-manager-pbs-setup-poll globus-gram-job-manager-pbs-setup-seg globus-gram-job-manager-scripts globus-gram-job-manager-scripts-doc globus-gram-job-manager-sge globus-gram-job-manager-sge-debuginfo globus-gram-job-manager-sge-setup-poll globus-gram-job-manager-sge-setup-seg globus-gram-protocol globus-gram-protocol-debuginfo globus-gram-protocol-devel globus-gram-protocol-doc globus-gridftp-server globus-gridftp-server-control globus-gridftp-server-control-debuginfo globus-gridftp-server-control-devel globus-gridftp-server-debuginfo globus-gridftp-server-devel globus-gridftp-server-progs globus-gridmap-callout-error globus-gridmap-callout-error-debuginfo globus-gridmap-callout-error-devel globus-gridmap-callout-error-doc globus-gsi-callback globus-gsi-callback-debuginfo globus-gsi-callback-devel globus-gsi-callback-doc globus-gsi-cert-utils globus-gsi-cert-utils-debuginfo globus-gsi-cert-utils-devel globus-gsi-cert-utils-doc globus-gsi-cert-utils-progs globus-gsi-credential globus-gsi-credential-debuginfo globus-gsi-credential-devel globus-gsi-credential-doc globus-gsi-openssl-error globus-gsi-openssl-error-debuginfo globus-gsi-openssl-error-devel globus-gsi-openssl-error-doc globus-gsi-proxy-core globus-gsi-proxy-core-debuginfo globus-gsi-proxy-core-devel globus-gsi-proxy-core-doc globus-gsi-proxy-ssl globus-gsi-proxy-ssl-debuginfo globus-gsi-proxy-ssl-devel globus-gsi-proxy-ssl-doc globus-gsi-sysconfig globus-gsi-sysconfig-debuginfo globus-gsi-sysconfig-devel globus-gsi-sysconfig-doc globus-gssapi-error globus-gssapi-error-debuginfo globus-gssapi-error-devel globus-gssapi-error-doc globus-gssapi-gsi globus-gssapi-gsi-debuginfo globus-gssapi-gsi-devel globus-gssapi-gsi-doc globus-gss-assist globus-gss-assist-debuginfo globus-gss-assist-devel globus-gss-assist-doc globus-gss-assist-progs globus-io globus-io-debuginfo globus-io-devel globus-openssl-module globus-openssl-module-debuginfo globus-openssl-module-devel globus-openssl-module-doc globus-proxy-utils globus-proxy-utils-debuginfo globus-rsl globus-rsl-debuginfo globus-rsl-devel globus-rsl-doc globus-scheduler-event-generator globus-scheduler-event-generator-debuginfo globus-scheduler-event-generator-devel globus-scheduler-event-generator-doc globus-scheduler-event-generator-progs globus-simple-ca globus-usage globus-usage-debuginfo globus-usage-devel globus-xio globus-xio-debuginfo globus-xio-devel globus-xio-doc globus-xio-gsi-driver globus-xio-gsi-driver-debuginfo globus-xio-gsi-driver-devel globus-xio-gsi-driver-doc globus-xioperf globus-xioperf-debuginfo globus-xio-pipe-driver globus-xio-pipe-driver-debuginfo globus-xio-pipe-driver-devel globus-xio-popen-driver globus-xio-popen-driver-debuginfo globus-xio-popen-driver-devel globus-xio-udt-driver globus-xio-udt-driver-debuginfo globus-xio-udt-driver-devel gratia-debuginfo gratia-probe-bdii-status gratia-probe-common gratia-probe-condor gratia-probe-condor-events gratia-probe-dcache-storage gratia-probe-dcache-storagegroup gratia-probe-dcache-transfer gratia-probe-debuginfo gratia-probe-enstore-storage gratia-probe-enstore-tapedrive gratia-probe-enstore-transfer gratia-probe-glexec gratia-probe-glideinwms gratia-probe-gram gratia-probe-gridftp-transfer gratia-probe-hadoop-storage gratia-probe-lsf gratia-probe-metric gratia-probe-onevm gratia-probe-pbs-lsf gratia-probe-psacct gratia-probe-services gratia-probe-sge gratia-probe-slurm gratia-probe-xrootd-storage gratia-probe-xrootd-transfer gratia-reporting-email gratia-service gridftp-hdfs gridftp-hdfs-debuginfo gsi-openssh gsi-openssh-clients gsi-openssh-debuginfo gsi-openssh-server gums gums-client gums-service hadoop hadoop-client hadoop-conf-pseudo hadoop-debuginfo hadoop-doc hadoop-hdfs hadoop-hdfs-datanode hadoop-hdfs-fuse hadoop-hdfs-fuse-selinux hadoop-hdfs-journalnode hadoop-hdfs-namenode hadoop-hdfs-secondarynamenode hadoop-hdfs-zkfc hadoop-httpfs hadoop-libhdfs hadoop-mapreduce hadoop-yarn htcondor-ce htcondor-ce-client htcondor-ce-collector htcondor-ce-condor htcondor-ce-debuginfo htcondor-ce-lsf htcondor-ce-pbs htcondor-ce-sge I2util I2util-debuginfo igtf-ca-certs javascriptrrd jetty jetty-ajp jetty-annotations jetty-client jetty-continuation jetty-deploy jetty-http jetty-io jetty-jmx jetty-jndi jetty-overlay-deployer jetty-plus jetty-policy jetty-rewrite jetty-security jetty-server jetty-servlet jetty-servlets jetty-util jetty-webapp jetty-websocket jetty-xml jglobus joda-time joda-time-javadoc koji koji-builder koji-hub koji-hub-plugins koji-utils koji-vm koji-web lcas-lcmaps-gt4-interface lcas-lcmaps-gt4-interface-debuginfo lcmaps lcmaps-common-devel lcmaps-debuginfo lcmaps-devel lcmaps-plugins-basic lcmaps-plugins-basic-debuginfo lcmaps-plugins-basic-ldap lcmaps-plugins-glexec-tracking lcmaps-plugins-glexec-tracking-debuginfo lcmaps-plugins-gums-client lcmaps-plugins-scas-client lcmaps-plugins-scas-client-debuginfo lcmaps-plugins-verify-proxy lcmaps-plugins-verify-proxy-debuginfo lcmaps-without-gsi lcmaps-without-gsi-devel llrun llrun-debuginfo mash mkgltempdir myproxy myproxy-admin myproxy-debuginfo myproxy-devel myproxy-doc myproxy-libs myproxy-server myproxy-voms ndt ndt-client ndt-debuginfo ndt-server netlogger nuttcp nuttcp-debuginfo osg-base-ce osg-base-ce-condor osg-base-ce-lsf osg-base-ce-pbs osg-base-ce-sge osg-base-ce-slurm osg-build osg-ca-certs osg-ca-certs-updater osg-ca-scripts osg-ce osg-ce-condor osg-ce-lsf osg-ce-pbs osg-cert-scripts osg-ce-sge osg-ce-slurm osg-cleanup osg-configure osg-configure-ce osg-configure-cemon osg-configure-condor osg-configure-gateway osg-configure-gip osg-configure-gratia osg-configure-infoservices osg-configure-lsf osg-configure-managedfork osg-configure-misc osg-configure-monalisa osg-configure-network osg-configure-pbs osg-configure-rsv osg-configure-sge osg-configure-slurm osg-configure-squid osg-configure-tests osg-control osg-gridftp osg-gridftp-hdfs osg-gridftp-xrootd osg-gums osg-gums-config osg-htcondor-ce osg-htcondor-ce-condor osg-htcondor-ce-lsf osg-htcondor-ce-pbs osg-htcondor-ce-sge osg-htcondor-ce-slurm osg-info-services osg-java7-compat osg-java7-compat-openjdk osg-java7-devel-compat osg-java7-devel-compat-openjdk osg-oasis osg-pki-tools osg-pki-tools-tests osg-release osg-release-itb osg-se-bestman osg-se-bestman-xrootd osg-se-hadoop osg-se-hadoop-client osg-se-hadoop-datanode osg-se-hadoop-gridftp osg-se-hadoop-namenode osg-se-hadoop-secondarynamenode osg-se-hadoop-srm osg-system-profiler osg-system-profiler-viewer osg-test osg-tested-internal osg-version osg-vo-map osg-voms osg-webapp-common osg-wn-client osg-wn-client-glexec owamp owamp-client owamp-debuginfo owamp-server pegasus pegasus-debuginfo privilege-xacml rsv rsv-consumers rsv-core rsv-metrics rsv-perfsonar rsv-vo-gwms stashcache-cache-server stashcache-daemon stashcache-origin-server uberftp uberftp-debuginfo vo-client vo-client-edgmkgridmap voms voms-admin-client voms-admin-server voms-api-java voms-api-java-javadoc voms-clients-cpp voms-debuginfo voms-devel voms-doc voms-mysql-plugin voms-mysql-plugin-debuginfo voms-server web100_userland web100_userland-debuginfo xacml xacml-debuginfo xacml-devel xrootd xrootd-client xrootd-client-devel xrootd-client-libs xrootd-debuginfo xrootd-devel xrootd-doc xrootd-dsi xrootd-dsi-debuginfo xrootd-fuse xrootd-hdfs xrootd-hdfs-debuginfo xrootd-hdfs-devel xrootd-lcmaps xrootd-lcmaps-debuginfo xrootd-libs xrootd-private-devel xrootd-python xrootd-selinux xrootd-server xrootd-server-devel xrootd-server-libs xrootd-status-probe xrootd-status-probe-debuginfo xrootd-voms-plugin xrootd-voms-plugin-debuginfo xrootd-voms-plugin-devel zookeeper zookeeper-server

If you wish to only update the RPMs that changed, the set of RPMs is:

#### Enterprise Linux 6

``` file
bestman2-2.3.0-25.osg33.el6
bestman2-client-2.3.0-25.osg33.el6
bestman2-client-libs-2.3.0-25.osg33.el6
bestman2-common-libs-2.3.0-25.osg33.el6
bestman2-server-2.3.0-25.osg33.el6
bestman2-server-dep-libs-2.3.0-25.osg33.el6
bestman2-server-libs-2.3.0-25.osg33.el6
bestman2-tester-2.3.0-25.osg33.el6
bestman2-tester-libs-2.3.0-25.osg33.el6
bigtop-jsvc-0.3.0-1.1.osg33.el6
bigtop-jsvc-debuginfo-0.3.0-1.1.osg33.el6
bigtop-utils-0.4+300-1.cdh4.0.1.p0.3.osg33.el6
blahp-1.18.13.bosco-3.osg33.el6
blahp-debuginfo-1.18.13.bosco-3.osg33.el6
bwctl-1.4-7.osg33.el6
bwctl-client-1.4-7.osg33.el6
bwctl-debuginfo-1.4-7.osg33.el6
bwctl-devel-1.4-7.osg33.el6
bwctl-server-1.4-7.osg33.el6
cctools-4.4.0-1.osg33.el6
cctools-chirp-4.4.0-1.osg33.el6
cctools-debuginfo-4.4.0-1.osg33.el6
cctools-doc-4.4.0-1.osg33.el6
cctools-dttools-4.4.0-1.osg33.el6
cctools-makeflow-4.4.0-1.osg33.el6
cctools-parrot-4.4.0-1.osg33.el6
cctools-resource_monitor-4.4.0-1.osg33.el6
cctools-sand-4.4.0-1.osg33.el6
cctools-wavefront-4.4.0-1.osg33.el6
cctools-work_queue-4.4.0-1.osg33.el6
cilogon-openid-ca-cert-1.1-2.osg33.el6
cilogon-osg-ca-cert-1.0-1.osg33.el6
cog-jglobus-axis-1.8.0-7.osg33.el6
condor-8.3.6-1.4.osg33.el6
condor-all-8.3.6-1.4.osg33.el6
condor-bosco-8.3.6-1.4.osg33.el6
condor-classads-8.3.6-1.4.osg33.el6
condor-classads-devel-8.3.6-1.4.osg33.el6
condor-cream-gahp-8.3.6-1.4.osg33.el6
condor-cron-1.0.9-4.osg33.el6
condor-debuginfo-8.3.6-1.4.osg33.el6
condor-kbdd-8.3.6-1.4.osg33.el6
condor-procd-8.3.6-1.4.osg33.el6
condor-python-8.3.6-1.4.osg33.el6
condor-std-universe-8.3.6-1.4.osg33.el6
condor-test-8.3.6-1.4.osg33.el6
condor-vm-gahp-8.3.6-1.4.osg33.el6
cvmfs-2.1.20-1.osg33.el6
cvmfs-config-osg-1.1-7.osg33.el6
cvmfs-devel-2.1.20-1.osg33.el6
cvmfs-server-2.1.20-1.osg33.el6
cvmfs-unittests-2.1.20-1.osg33.el6
edg-mkgridmap-4.0.2-1.osg33.el6
emi-trustmanager-3.0.3-8.osg33.el6
emi-trustmanager-axis-1.0.1-1.4.osg33.el6
emi-trustmanager-tomcat-3.0.0-10.osg33.el6
frontier-squid-2.7.STABLE9-24.1.osg33.el6
frontier-squid-debuginfo-2.7.STABLE9-24.1.osg33.el6
gfal2-plugin-xrootd-0.3.pre1-2.1.osg33.el6
gfal2-plugin-xrootd-debuginfo-0.3.pre1-2.1.osg33.el6
gip-1.3.11-6.osg33.el6
glexec-0.9.11-1.1.osg33.el6
glexec-debuginfo-0.9.11-1.1.osg33.el6
glexec-wrapper-scripts-0.0.7-1.1.osg33.el6
glideinwms-3.2.10-1.1.osg33.el6
glideinwms-factory-3.2.10-1.1.osg33.el6
glideinwms-factory-condor-3.2.10-1.1.osg33.el6
glideinwms-glidecondor-tools-3.2.10-1.1.osg33.el6
glideinwms-libs-3.2.10-1.1.osg33.el6
glideinwms-minimal-condor-3.2.10-1.1.osg33.el6
glideinwms-usercollector-3.2.10-1.1.osg33.el6
glideinwms-userschedd-3.2.10-1.1.osg33.el6
glideinwms-vofrontend-3.2.10-1.1.osg33.el6
glideinwms-vofrontend-standalone-3.2.10-1.1.osg33.el6
glite-build-common-cpp-3.3.0.2-1.osg33.el6
glite-ce-cream-client-api-c-1.14.0-4.10.osg33.el6
glite-ce-cream-client-api-c-debuginfo-1.14.0-4.10.osg33.el6
glite-ce-cream-client-devel-1.14.0-4.10.osg33.el6
glite-ce-cream-utils-1.2.0-4.3.osg33.el6
glite-lbjp-common-gsoap-plugin-3.1.2-2.2.osg33.el6
glite-lbjp-common-gsoap-plugin-debuginfo-3.1.2-2.2.osg33.el6
glite-lbjp-common-gsoap-plugin-devel-3.1.2-2.2.osg33.el6
glite-lbjp-common-gss-3.1.3-2.2.osg33.el6
glite-lbjp-common-gss-debuginfo-3.1.3-2.2.osg33.el6
glite-lbjp-common-gss-devel-3.1.3-2.2.osg33.el6
globus-authz-3.10-1.osg33.el6
globus-authz-callout-error-3.5-1.osg33.el6
globus-authz-callout-error-debuginfo-3.5-1.osg33.el6
globus-authz-callout-error-devel-3.5-1.osg33.el6
globus-authz-callout-error-doc-3.5-1.osg33.el6
globus-authz-debuginfo-3.10-1.osg33.el6
globus-authz-devel-3.10-1.osg33.el6
globus-authz-doc-3.10-1.osg33.el6
globus-callout-3.13-1.osg33.el6
globus-callout-debuginfo-3.13-1.osg33.el6
globus-callout-devel-3.13-1.osg33.el6
globus-callout-doc-3.13-1.osg33.el6
globus-common-15.27-1.osg33.el6
globus-common-debuginfo-15.27-1.osg33.el6
globus-common-devel-15.27-1.osg33.el6
globus-common-doc-15.27-1.osg33.el6
globus-common-progs-15.27-1.osg33.el6
globus-ftp-client-8.19-1.2.osg33.el6
globus-ftp-client-debuginfo-8.19-1.2.osg33.el6
globus-ftp-client-devel-8.19-1.2.osg33.el6
globus-ftp-client-doc-8.19-1.2.osg33.el6
globus-ftp-control-6.6-1.1.osg33.el6
globus-ftp-control-debuginfo-6.6-1.1.osg33.el6
globus-ftp-control-devel-6.6-1.1.osg33.el6
globus-ftp-control-doc-6.6-1.1.osg33.el6
globus-gass-cache-9.5-1.osg33.el6
globus-gass-cache-debuginfo-9.5-1.osg33.el6
globus-gass-cache-devel-9.5-1.osg33.el6
globus-gass-cache-doc-9.5-1.osg33.el6
globus-gass-cache-program-6.5-1.osg33.el6
globus-gass-cache-program-debuginfo-6.5-1.osg33.el6
globus-gass-copy-9.13-1.osg33.el6
globus-gass-copy-debuginfo-9.13-1.osg33.el6
globus-gass-copy-devel-9.13-1.osg33.el6
globus-gass-copy-doc-9.13-1.osg33.el6
globus-gass-copy-progs-9.13-1.osg33.el6
globus-gass-server-ez-5.7-1.osg33.el6
globus-gass-server-ez-debuginfo-5.7-1.osg33.el6
globus-gass-server-ez-devel-5.7-1.osg33.el6
globus-gass-server-ez-progs-5.7-1.osg33.el6
globus-gass-transfer-8.8-1.osg33.el6
globus-gass-transfer-debuginfo-8.8-1.osg33.el6
globus-gass-transfer-devel-8.8-1.osg33.el6
globus-gass-transfer-doc-8.8-1.osg33.el6
globus-gatekeeper-10.9-1.2.osg33.el6
globus-gatekeeper-debuginfo-10.9-1.2.osg33.el6
globus-gfork-4.7-1.osg33.el6
globus-gfork-debuginfo-4.7-1.osg33.el6
globus-gfork-devel-4.7-1.osg33.el6
globus-gfork-progs-4.7-1.osg33.el6
globus-gram-audit-4.4-1.osg33.el6
globus-gram-client-13.12-1.osg33.el6
globus-gram-client-debuginfo-13.12-1.osg33.el6
globus-gram-client-devel-13.12-1.osg33.el6
globus-gram-client-doc-13.12-1.osg33.el6
globus-gram-client-tools-11.7-1.osg33.el6
globus-gram-client-tools-debuginfo-11.7-1.osg33.el6
globus-gram-job-manager-14.25-1.2.osg33.el6
globus-gram-job-manager-callout-error-3.5-1.osg33.el6
globus-gram-job-manager-callout-error-debuginfo-3.5-1.osg33.el6
globus-gram-job-manager-callout-error-devel-3.5-1.osg33.el6
globus-gram-job-manager-callout-error-doc-3.5-1.osg33.el6
globus-gram-job-manager-condor-2.5-1.1.osg33.el6
globus-gram-job-manager-debuginfo-14.25-1.2.osg33.el6
globus-gram-job-manager-fork-2.4-1.1.osg33.el6
globus-gram-job-manager-fork-debuginfo-2.4-1.1.osg33.el6
globus-gram-job-manager-fork-setup-poll-2.4-1.1.osg33.el6
globus-gram-job-manager-fork-setup-seg-2.4-1.1.osg33.el6
globus-gram-job-manager-lsf-2.6-1.2.osg33.el6
globus-gram-job-manager-lsf-debuginfo-2.6-1.2.osg33.el6
globus-gram-job-manager-lsf-setup-poll-2.6-1.2.osg33.el6
globus-gram-job-manager-lsf-setup-seg-2.6-1.2.osg33.el6
globus-gram-job-manager-managedfork-0.2-1.osg33.el6
globus-gram-job-manager-pbs-2.4-2.1.osg33.el6
globus-gram-job-manager-pbs-debuginfo-2.4-2.1.osg33.el6
globus-gram-job-manager-pbs-setup-poll-2.4-2.1.osg33.el6
globus-gram-job-manager-pbs-setup-seg-2.4-2.1.osg33.el6
globus-gram-job-manager-scripts-6.7-1.osg33.el6
globus-gram-job-manager-scripts-doc-6.7-1.osg33.el6
globus-gram-job-manager-sge-2.5-1.1.osg33.el6
globus-gram-job-manager-sge-debuginfo-2.5-1.1.osg33.el6
globus-gram-job-manager-sge-setup-poll-2.5-1.1.osg33.el6
globus-gram-job-manager-sge-setup-seg-2.5-1.1.osg33.el6
globus-gram-protocol-12.12-2.osg33.el6
globus-gram-protocol-debuginfo-12.12-2.osg33.el6
globus-gram-protocol-devel-12.12-2.osg33.el6
globus-gram-protocol-doc-12.12-2.osg33.el6
globus-gridftp-server-7.20-1.1.osg33.el6
globus-gridftp-server-control-3.6-1.osg33.el6
globus-gridftp-server-control-debuginfo-3.6-1.osg33.el6
globus-gridftp-server-control-devel-3.6-1.osg33.el6
globus-gridftp-server-debuginfo-7.20-1.1.osg33.el6
globus-gridftp-server-devel-7.20-1.1.osg33.el6
globus-gridftp-server-progs-7.20-1.1.osg33.el6
globus-gridmap-callout-error-2.4-1.osg33.el6
globus-gridmap-callout-error-debuginfo-2.4-1.osg33.el6
globus-gridmap-callout-error-devel-2.4-1.osg33.el6
globus-gridmap-callout-error-doc-2.4-1.osg33.el6
globus-gsi-callback-5.7-1.osg33.el6
globus-gsi-callback-debuginfo-5.7-1.osg33.el6
globus-gsi-callback-devel-5.7-1.osg33.el6
globus-gsi-callback-doc-5.7-1.osg33.el6
globus-gsi-cert-utils-9.10-1.osg33.el6
globus-gsi-cert-utils-debuginfo-9.10-1.osg33.el6
globus-gsi-cert-utils-devel-9.10-1.osg33.el6
globus-gsi-cert-utils-doc-9.10-1.osg33.el6
globus-gsi-cert-utils-progs-9.10-1.osg33.el6
globus-gsi-credential-7.7-1.osg33.el6
globus-gsi-credential-debuginfo-7.7-1.osg33.el6
globus-gsi-credential-devel-7.7-1.osg33.el6
globus-gsi-credential-doc-7.7-1.osg33.el6
globus-gsi-openssl-error-3.5-1.osg33.el6
globus-gsi-openssl-error-debuginfo-3.5-1.osg33.el6
globus-gsi-openssl-error-devel-3.5-1.osg33.el6
globus-gsi-openssl-error-doc-3.5-1.osg33.el6
globus-gsi-proxy-core-7.7-1.osg33.el6
globus-gsi-proxy-core-debuginfo-7.7-1.osg33.el6
globus-gsi-proxy-core-devel-7.7-1.osg33.el6
globus-gsi-proxy-core-doc-7.7-1.osg33.el6
globus-gsi-proxy-ssl-5.7-1.osg33.el6
globus-gsi-proxy-ssl-debuginfo-5.7-1.osg33.el6
globus-gsi-proxy-ssl-devel-5.7-1.osg33.el6
globus-gsi-proxy-ssl-doc-5.7-1.osg33.el6
globus-gsi-sysconfig-6.8-1.osg33.el6
globus-gsi-sysconfig-debuginfo-6.8-1.osg33.el6
globus-gsi-sysconfig-devel-6.8-1.osg33.el6
globus-gsi-sysconfig-doc-6.8-1.osg33.el6
globus-gssapi-error-5.4-1.osg33.el6
globus-gssapi-error-debuginfo-5.4-1.osg33.el6
globus-gssapi-error-devel-5.4-1.osg33.el6
globus-gssapi-error-doc-5.4-1.osg33.el6
globus-gssapi-gsi-11.18-1.osg33.el6
globus-gssapi-gsi-debuginfo-11.18-1.osg33.el6
globus-gssapi-gsi-devel-11.18-1.osg33.el6
globus-gssapi-gsi-doc-11.18-1.osg33.el6
globus-gss-assist-10.13-1.osg33.el6
globus-gss-assist-debuginfo-10.13-1.osg33.el6
globus-gss-assist-devel-10.13-1.osg33.el6
globus-gss-assist-doc-10.13-1.osg33.el6
globus-gss-assist-progs-10.13-1.osg33.el6
globus-io-11.4-1.osg33.el6
globus-io-debuginfo-11.4-1.osg33.el6
globus-io-devel-11.4-1.osg33.el6
globus-openssl-module-4.6-1.osg33.el6
globus-openssl-module-debuginfo-4.6-1.osg33.el6
globus-openssl-module-devel-4.6-1.osg33.el6
globus-openssl-module-doc-4.6-1.osg33.el6
globus-proxy-utils-6.9-1.osg33.el6
globus-proxy-utils-debuginfo-6.9-1.osg33.el6
globus-rsl-10.9-1.osg33.el6
globus-rsl-debuginfo-10.9-1.osg33.el6
globus-rsl-devel-10.9-1.osg33.el6
globus-rsl-doc-10.9-1.osg33.el6
globus-scheduler-event-generator-5.10-2.1.osg33.el6
globus-scheduler-event-generator-debuginfo-5.10-2.1.osg33.el6
globus-scheduler-event-generator-devel-5.10-2.1.osg33.el6
globus-scheduler-event-generator-doc-5.10-2.1.osg33.el6
globus-scheduler-event-generator-progs-5.10-2.1.osg33.el6
globus-simple-ca-4.18-1.osg33.el6
globus-usage-4.4-1.osg33.el6
globus-usage-debuginfo-4.4-1.osg33.el6
globus-usage-devel-4.4-1.osg33.el6
globus-xio-5.7-1.1.osg33.el6
globus-xio-debuginfo-5.7-1.1.osg33.el6
globus-xio-devel-5.7-1.1.osg33.el6
globus-xio-doc-5.7-1.1.osg33.el6
globus-xio-gsi-driver-3.7-1.osg33.el6
globus-xio-gsi-driver-debuginfo-3.7-1.osg33.el6
globus-xio-gsi-driver-devel-3.7-1.osg33.el6
globus-xio-gsi-driver-doc-3.7-1.osg33.el6
globus-xioperf-4.4-1.osg33.el6
globus-xioperf-debuginfo-4.4-1.osg33.el6
globus-xio-pipe-driver-3.7-1.osg33.el6
globus-xio-pipe-driver-debuginfo-3.7-1.osg33.el6
globus-xio-pipe-driver-devel-3.7-1.osg33.el6
globus-xio-popen-driver-3.5-1.osg33.el6
globus-xio-popen-driver-debuginfo-3.5-1.osg33.el6
globus-xio-popen-driver-devel-3.5-1.osg33.el6
globus-xio-udt-driver-1.16-1.osg33.el6
globus-xio-udt-driver-debuginfo-1.16-1.osg33.el6
globus-xio-udt-driver-devel-1.16-1.osg33.el6
gratia-1.16.2-1.osg33.el6
gratia-debuginfo-1.16.2-1.osg33.el6
gratia-probe-1.14.2-6.osg33.el6
gratia-probe-bdii-status-1.14.2-6.osg33.el6
gratia-probe-common-1.14.2-6.osg33.el6
gratia-probe-condor-1.14.2-6.osg33.el6
gratia-probe-condor-events-1.14.2-6.osg33.el6
gratia-probe-dcache-storage-1.14.2-6.osg33.el6
gratia-probe-dcache-storagegroup-1.14.2-6.osg33.el6
gratia-probe-dcache-transfer-1.14.2-6.osg33.el6
gratia-probe-debuginfo-1.14.2-6.osg33.el6
gratia-probe-enstore-storage-1.14.2-6.osg33.el6
gratia-probe-enstore-tapedrive-1.14.2-6.osg33.el6
gratia-probe-enstore-transfer-1.14.2-6.osg33.el6
gratia-probe-glexec-1.14.2-6.osg33.el6
gratia-probe-glideinwms-1.14.2-6.osg33.el6
gratia-probe-gram-1.14.2-6.osg33.el6
gratia-probe-gridftp-transfer-1.14.2-6.osg33.el6
gratia-probe-hadoop-storage-1.14.2-6.osg33.el6
gratia-probe-lsf-1.14.2-6.osg33.el6
gratia-probe-metric-1.14.2-6.osg33.el6
gratia-probe-onevm-1.14.2-6.osg33.el6
gratia-probe-pbs-lsf-1.14.2-6.osg33.el6
gratia-probe-psacct-1.14.2-6.osg33.el6
gratia-probe-services-1.14.2-6.osg33.el6
gratia-probe-sge-1.14.2-6.osg33.el6
gratia-probe-slurm-1.14.2-6.osg33.el6
gratia-probe-xrootd-storage-1.14.2-6.osg33.el6
gratia-probe-xrootd-transfer-1.14.2-6.osg33.el6
gratia-reporting-email-1.15.1-1.osg33.el6
gratia-service-1.16.2-1.osg33.el6
gridftp-hdfs-0.5.4-19.osg33.el6
gridftp-hdfs-debuginfo-0.5.4-19.osg33.el6
gsi-openssh-5.7-1.1.osg33.el6
gsi-openssh-clients-5.7-1.1.osg33.el6
gsi-openssh-debuginfo-5.7-1.1.osg33.el6
gsi-openssh-server-5.7-1.1.osg33.el6
gums-1.4.4-3.osg33.el6
gums-client-1.4.4-3.osg33.el6
gums-service-1.4.4-3.osg33.el6
hadoop-2.0.0+545-1.cdh4.1.1.p0.20.osg33.el6
hadoop-client-2.0.0+545-1.cdh4.1.1.p0.20.osg33.el6
hadoop-conf-pseudo-2.0.0+545-1.cdh4.1.1.p0.20.osg33.el6
hadoop-debuginfo-2.0.0+545-1.cdh4.1.1.p0.20.osg33.el6
hadoop-doc-2.0.0+545-1.cdh4.1.1.p0.20.osg33.el6
hadoop-hdfs-2.0.0+545-1.cdh4.1.1.p0.20.osg33.el6
hadoop-hdfs-datanode-2.0.0+545-1.cdh4.1.1.p0.20.osg33.el6
hadoop-hdfs-fuse-2.0.0+545-1.cdh4.1.1.p0.20.osg33.el6
hadoop-hdfs-fuse-selinux-2.0.0+545-1.cdh4.1.1.p0.20.osg33.el6
hadoop-hdfs-journalnode-2.0.0+545-1.cdh4.1.1.p0.20.osg33.el6
hadoop-hdfs-namenode-2.0.0+545-1.cdh4.1.1.p0.20.osg33.el6
hadoop-hdfs-secondarynamenode-2.0.0+545-1.cdh4.1.1.p0.20.osg33.el6
hadoop-hdfs-zkfc-2.0.0+545-1.cdh4.1.1.p0.20.osg33.el6
hadoop-httpfs-2.0.0+545-1.cdh4.1.1.p0.20.osg33.el6
hadoop-libhdfs-2.0.0+545-1.cdh4.1.1.p0.20.osg33.el6
hadoop-mapreduce-2.0.0+545-1.cdh4.1.1.p0.20.osg33.el6
hadoop-yarn-2.0.0+545-1.cdh4.1.1.p0.20.osg33.el6
htcondor-ce-1.14-4.osg33.el6
htcondor-ce-client-1.14-4.osg33.el6
htcondor-ce-collector-1.14-4.osg33.el6
htcondor-ce-condor-1.14-4.osg33.el6
htcondor-ce-debuginfo-1.14-4.osg33.el6
htcondor-ce-lsf-1.14-4.osg33.el6
htcondor-ce-pbs-1.14-4.osg33.el6
htcondor-ce-sge-1.14-4.osg33.el6
I2util-1.1-2.osg33.el6
I2util-debuginfo-1.1-2.osg33.el6
igtf-ca-certs-1.65-1.osg33.el6
javascriptrrd-1.1.1-1.osg33.el6
jetty-8.1.4.v20120524-2.osg33.el6
jetty-ajp-8.1.4.v20120524-2.osg33.el6
jetty-annotations-8.1.4.v20120524-2.osg33.el6
jetty-client-8.1.4.v20120524-2.osg33.el6
jetty-continuation-8.1.4.v20120524-2.osg33.el6
jetty-deploy-8.1.4.v20120524-2.osg33.el6
jetty-http-8.1.4.v20120524-2.osg33.el6
jetty-io-8.1.4.v20120524-2.osg33.el6
jetty-jmx-8.1.4.v20120524-2.osg33.el6
jetty-jndi-8.1.4.v20120524-2.osg33.el6
jetty-overlay-deployer-8.1.4.v20120524-2.osg33.el6
jetty-plus-8.1.4.v20120524-2.osg33.el6
jetty-policy-8.1.4.v20120524-2.osg33.el6
jetty-rewrite-8.1.4.v20120524-2.osg33.el6
jetty-security-8.1.4.v20120524-2.osg33.el6
jetty-server-8.1.4.v20120524-2.osg33.el6
jetty-servlet-8.1.4.v20120524-2.osg33.el6
jetty-servlets-8.1.4.v20120524-2.osg33.el6
jetty-util-8.1.4.v20120524-2.osg33.el6
jetty-webapp-8.1.4.v20120524-2.osg33.el6
jetty-websocket-8.1.4.v20120524-2.osg33.el6
jetty-xml-8.1.4.v20120524-2.osg33.el6
jglobus-2.0.6-4.osg33.el6
joda-time-1.5.2-7.2.tzdata2008d.osg33.el6
joda-time-javadoc-1.5.2-7.2.tzdata2008d.osg33.el6
koji-1.6.0-10.osg33.el6
koji-builder-1.6.0-10.osg33.el6
koji-hub-1.6.0-10.osg33.el6
koji-hub-plugins-1.6.0-10.osg33.el6
koji-utils-1.6.0-10.osg33.el6
koji-vm-1.6.0-10.osg33.el6
koji-web-1.6.0-10.osg33.el6
lcas-lcmaps-gt4-interface-0.2.6-1.1.osg33.el6
lcas-lcmaps-gt4-interface-debuginfo-0.2.6-1.1.osg33.el6
lcmaps-1.6.6-1.1.osg33.el6
lcmaps-common-devel-1.6.6-1.1.osg33.el6
lcmaps-debuginfo-1.6.6-1.1.osg33.el6
lcmaps-devel-1.6.6-1.1.osg33.el6
lcmaps-plugins-basic-1.7.0-2.osg33.el6
lcmaps-plugins-basic-debuginfo-1.7.0-2.osg33.el6
lcmaps-plugins-basic-ldap-1.7.0-2.osg33.el6
lcmaps-plugins-glexec-tracking-0.1.6-1.osg33.el6
lcmaps-plugins-glexec-tracking-debuginfo-0.1.6-1.osg33.el6
lcmaps-plugins-gums-client-0.0.2-4.osg33.el6
lcmaps-plugins-scas-client-0.5.5-1.osg33.el6
lcmaps-plugins-scas-client-debuginfo-0.5.5-1.osg33.el6
lcmaps-plugins-verify-proxy-1.5.7-1.osg33.el6
lcmaps-plugins-verify-proxy-debuginfo-1.5.7-1.osg33.el6
lcmaps-without-gsi-1.6.6-1.1.osg33.el6
lcmaps-without-gsi-devel-1.6.6-1.1.osg33.el6
llrun-0.1.3-1.3.osg33.el6
llrun-debuginfo-0.1.3-1.3.osg33.el6
mash-0.5.22-3.osg33.el6
mkgltempdir-0.0.5-1.1.osg33.el6
myproxy-6.1.12-1.osg33.el6
myproxy-admin-6.1.12-1.osg33.el6
myproxy-debuginfo-6.1.12-1.osg33.el6
myproxy-devel-6.1.12-1.osg33.el6
myproxy-doc-6.1.12-1.osg33.el6
myproxy-libs-6.1.12-1.osg33.el6
myproxy-server-6.1.12-1.osg33.el6
myproxy-voms-6.1.12-1.osg33.el6
ndt-3.6.5-3.osg33.el6
ndt-client-3.6.5-3.osg33.el6
ndt-debuginfo-3.6.5-3.osg33.el6
ndt-server-3.6.5-3.osg33.el6
netlogger-4.2.0-9.osg33.el6
nuttcp-6.1.2-1.osg33.el6
nuttcp-debuginfo-6.1.2-1.osg33.el6
osg-base-ce-3.3-3.osg33.el6
osg-base-ce-condor-3.3-3.osg33.el6
osg-base-ce-lsf-3.3-3.osg33.el6
osg-base-ce-pbs-3.3-3.osg33.el6
osg-base-ce-sge-3.3-3.osg33.el6
osg-base-ce-slurm-3.3-3.osg33.el6
osg-build-1.6.0-2.osg33.el6
osg-ca-certs-1.47-1.osg33.el6
osg-ca-certs-updater-1.0-1.osg33.el6
osg-ca-scripts-1.1.5-2.osg33.el6
osg-ce-3.3-3.osg33.el6
osg-ce-condor-3.3-3.osg33.el6
osg-ce-lsf-3.3-3.osg33.el6
osg-ce-pbs-3.3-3.osg33.el6
osg-cert-scripts-2.7.2-2.osg33.el6
osg-ce-sge-3.3-3.osg33.el6
osg-ce-slurm-3.3-3.osg33.el6
osg-cleanup-1.7.2-1.osg33.el6
osg-configure-1.1.1-1.osg33.el6
osg-configure-ce-1.1.1-1.osg33.el6
osg-configure-cemon-1.1.1-1.osg33.el6
osg-configure-condor-1.1.1-1.osg33.el6
osg-configure-gateway-1.1.1-1.osg33.el6
osg-configure-gip-1.1.1-1.osg33.el6
osg-configure-gratia-1.1.1-1.osg33.el6
osg-configure-infoservices-1.1.1-1.osg33.el6
osg-configure-lsf-1.1.1-1.osg33.el6
osg-configure-managedfork-1.1.1-1.osg33.el6
osg-configure-misc-1.1.1-1.osg33.el6
osg-configure-monalisa-1.1.1-1.osg33.el6
osg-configure-network-1.1.1-1.osg33.el6
osg-configure-pbs-1.1.1-1.osg33.el6
osg-configure-rsv-1.1.1-1.osg33.el6
osg-configure-sge-1.1.1-1.osg33.el6
osg-configure-slurm-1.1.1-1.osg33.el6
osg-configure-squid-1.1.1-1.osg33.el6
osg-configure-tests-1.1.1-1.osg33.el6
osg-control-1.0.1-1.osg33.el6
osg-gridftp-3.3-2.osg33.el6
osg-gridftp-hdfs-3.3-2.osg33.el6
osg-gridftp-xrootd-3.3-2.osg33.el6
osg-gums-3.3-2.osg33.el6
osg-gums-config-61-1.osg33.el6
osg-htcondor-ce-3.3-3.osg33.el6
osg-htcondor-ce-condor-3.3-3.osg33.el6
osg-htcondor-ce-lsf-3.3-3.osg33.el6
osg-htcondor-ce-pbs-3.3-3.osg33.el6
osg-htcondor-ce-sge-3.3-3.osg33.el6
osg-htcondor-ce-slurm-3.3-3.osg33.el6
osg-info-services-1.0.2-1.osg33.el6
osg-java7-compat-1.0-1.osg33.el6
osg-java7-compat-openjdk-1.0-1.osg33.el6
osg-java7-devel-compat-1.0-1.osg33.el6
osg-java7-devel-compat-openjdk-1.0-1.osg33.el6
osg-oasis-5-2.osg33.el6
osg-pki-tools-1.2.12-1.osg33.el6
osg-pki-tools-tests-1.2.12-1.osg33.el6
osg-release-3.3-2.osg33.el6
osg-release-itb-3.3-2.osg33.el6
osg-se-bestman-3.3-2.osg33.el6
osg-se-bestman-xrootd-3.3-2.osg33.el6
osg-se-hadoop-3.3-2.osg33.el6
osg-se-hadoop-client-3.3-2.osg33.el6
osg-se-hadoop-datanode-3.3-2.osg33.el6
osg-se-hadoop-gridftp-3.3-2.osg33.el6
osg-se-hadoop-namenode-3.3-2.osg33.el6
osg-se-hadoop-secondarynamenode-3.3-2.osg33.el6
osg-se-hadoop-srm-3.3-2.osg33.el6
osg-system-profiler-1.2.0-1.osg33.el6
osg-system-profiler-viewer-1.2.0-1.osg33.el6
osg-test-1.4.26-1.osg33.el6
osg-tested-internal-3.3-1.osg33.el6
osg-version-3.3.0-1.osg33.el6
osg-vo-map-0.0.1-1.osg33.el6
osg-voms-3.3-1.osg33.el6
osg-webapp-common-1-2.osg33.el6
osg-wn-client-3.3-5.osg33.el6
osg-wn-client-glexec-3.3-5.osg33.el6
owamp-3.2rc4-2.osg33.el6
owamp-client-3.2rc4-2.osg33.el6
owamp-debuginfo-3.2rc4-2.osg33.el6
owamp-server-3.2rc4-2.osg33.el6
pegasus-4.3.1-1.3.osg33.el6
pegasus-debuginfo-4.3.1-1.3.osg33.el6
privilege-xacml-2.6.4-1.osg33.el6
rsv-3.10.2-1.osg33.el6
rsv-consumers-3.10.2-1.osg33.el6
rsv-core-3.10.2-1.osg33.el6
rsv-metrics-3.10.2-1.osg33.el6
rsv-perfsonar-1.0.19-1.osg33.el6
rsv-vo-gwms-1.0.1-1.osg33.el6
stashcache-0.3-4.osg33.el6
stashcache-cache-server-0.3-4.osg33.el6
stashcache-daemon-0.2-1.osg33.el6
stashcache-daemon-0.3-4.osg33.el6
stashcache-origin-server-0.3-4.osg33.el6
uberftp-2.8-2.1.osg33.el6
uberftp-debuginfo-2.8-2.1.osg33.el6
vo-client-61-1.osg33.el6
vo-client-edgmkgridmap-61-1.osg33.el6
voms-2.0.12-3.osg33.el6
voms-admin-client-2.0.17-1.1.osg33.el6
voms-admin-server-2.7.0-1.14.osg33.el6
voms-api-java-2.0.8-1.6.osg33.el6
voms-api-java-javadoc-2.0.8-1.6.osg33.el6
voms-clients-cpp-2.0.12-3.osg33.el6
voms-debuginfo-2.0.12-3.osg33.el6
voms-devel-2.0.12-3.osg33.el6
voms-doc-2.0.12-3.osg33.el6
voms-mysql-plugin-3.1.6-1.1.osg33.el6
voms-mysql-plugin-debuginfo-3.1.6-1.1.osg33.el6
voms-server-2.0.12-3.osg33.el6
web100_userland-1.7-6.osg33.el6
web100_userland-debuginfo-1.7-6.osg33.el6
xacml-1.5.0-1.osg33.el6
xacml-debuginfo-1.5.0-1.osg33.el6
xacml-devel-1.5.0-1.osg33.el6
xrootd-4.2.2-1.osg33.el6
xrootd-client-4.2.2-1.osg33.el6
xrootd-client-devel-4.2.2-1.osg33.el6
xrootd-client-libs-4.2.2-1.osg33.el6
xrootd-debuginfo-4.2.2-1.osg33.el6
xrootd-devel-4.2.2-1.osg33.el6
xrootd-doc-4.2.2-1.osg33.el6
xrootd-dsi-3.0.4-16.osg33.el6
xrootd-dsi-debuginfo-3.0.4-16.osg33.el6
xrootd-fuse-4.2.2-1.osg33.el6
xrootd-hdfs-1.8.4-4.osg33.el6
xrootd-hdfs-debuginfo-1.8.4-4.osg33.el6
xrootd-hdfs-devel-1.8.4-4.osg33.el6
xrootd-lcmaps-0.0.7-11.osg33.el6
xrootd-lcmaps-debuginfo-0.0.7-11.osg33.el6
xrootd-libs-4.2.2-1.osg33.el6
xrootd-private-devel-4.2.2-1.osg33.el6
xrootd-python-4.2.2-1.osg33.el6
xrootd-selinux-4.2.2-1.osg33.el6
xrootd-server-4.2.2-1.osg33.el6
xrootd-server-devel-4.2.2-1.osg33.el6
xrootd-server-libs-4.2.2-1.osg33.el6
xrootd-status-probe-0.0.3-11.osg33.el6
xrootd-status-probe-debuginfo-0.0.3-11.osg33.el6
xrootd-voms-plugin-0.2.0-1.6.osg33.el6
xrootd-voms-plugin-debuginfo-0.2.0-1.6.osg33.el6
xrootd-voms-plugin-devel-0.2.0-1.6.osg33.el6
zookeeper-3.4.3+15-1.cdh4.0.1.p0.3.osg33.el6
zookeeper-server-3.4.3+15-1.cdh4.0.1.p0.3.osg33.el6
```

#### Enterprise Linux 7

``` file
bigtop-jsvc-0.3.0-1.1.osg33.el7
bigtop-jsvc-debuginfo-0.3.0-1.1.osg33.el7
bigtop-utils-0.4+300-1.cdh4.0.1.p0.3.osg33.el7
blahp-1.18.13.bosco-3.osg33.el7
blahp-debuginfo-1.18.13.bosco-3.osg33.el7
bwctl-1.4-7.osg33.el7
bwctl-client-1.4-7.osg33.el7
bwctl-debuginfo-1.4-7.osg33.el7
bwctl-devel-1.4-7.osg33.el7
bwctl-server-1.4-7.osg33.el7
cctools-4.4.0-1.osg33.el7
cctools-chirp-4.4.0-1.osg33.el7
cctools-debuginfo-4.4.0-1.osg33.el7
cctools-doc-4.4.0-1.osg33.el7
cctools-dttools-4.4.0-1.osg33.el7
cctools-makeflow-4.4.0-1.osg33.el7
cctools-parrot-4.4.0-1.osg33.el7
cctools-resource_monitor-4.4.0-1.osg33.el7
cctools-sand-4.4.0-1.osg33.el7
cctools-wavefront-4.4.0-1.osg33.el7
cctools-work_queue-4.4.0-1.osg33.el7
cilogon-openid-ca-cert-1.1-2.osg33.el7
cilogon-osg-ca-cert-1.0-1.osg33.el7
cog-jglobus-axis-1.8.0-8.osg33.el7
condor-8.3.6-1.4.osg33.el7
condor-all-8.3.6-1.4.osg33.el7
condor-bosco-8.3.6-1.4.osg33.el7
condor-classads-8.3.6-1.4.osg33.el7
condor-classads-devel-8.3.6-1.4.osg33.el7
condor-cron-1.0.9-4.osg33.el7
condor-debuginfo-8.3.6-1.4.osg33.el7
condor-kbdd-8.3.6-1.4.osg33.el7
condor-procd-8.3.6-1.4.osg33.el7
condor-python-8.3.6-1.4.osg33.el7
condor-test-8.3.6-1.4.osg33.el7
condor-vm-gahp-8.3.6-1.4.osg33.el7
cvmfs-2.1.20-1.osg33.el7
cvmfs-config-osg-1.1-7.osg33.el7
cvmfs-devel-2.1.20-1.osg33.el7
cvmfs-server-2.1.20-1.osg33.el7
cvmfs-unittests-2.1.20-1.osg33.el7
edg-mkgridmap-4.0.2-1.osg33.el7
emi-trustmanager-3.0.3-8.osg33.el7
emi-trustmanager-axis-1.0.1-1.4.osg33.el7
frontier-squid-2.7.STABLE9-24.1.osg33.el7
frontier-squid-debuginfo-2.7.STABLE9-24.1.osg33.el7
gfal2-plugin-xrootd-0.3.pre1-2.1.osg33.el7
gfal2-plugin-xrootd-debuginfo-0.3.pre1-2.1.osg33.el7
gip-1.3.11-6.osg33.el7
glexec-0.9.11-1.1.osg33.el7
glexec-debuginfo-0.9.11-1.1.osg33.el7
glexec-wrapper-scripts-0.0.7-1.1.osg33.el7
glideinwms-3.2.10-1.1.osg33.el7
glideinwms-factory-3.2.10-1.1.osg33.el7
glideinwms-factory-condor-3.2.10-1.1.osg33.el7
glideinwms-glidecondor-tools-3.2.10-1.1.osg33.el7
glideinwms-libs-3.2.10-1.1.osg33.el7
glideinwms-minimal-condor-3.2.10-1.1.osg33.el7
glideinwms-usercollector-3.2.10-1.1.osg33.el7
glideinwms-userschedd-3.2.10-1.1.osg33.el7
glideinwms-vofrontend-3.2.10-1.1.osg33.el7
glideinwms-vofrontend-standalone-3.2.10-1.1.osg33.el7
glite-build-common-cpp-3.3.0.2-1.osg33.el7
glite-lbjp-common-gsoap-plugin-3.1.2-2.2.osg33.el7
glite-lbjp-common-gsoap-plugin-debuginfo-3.1.2-2.2.osg33.el7
glite-lbjp-common-gsoap-plugin-devel-3.1.2-2.2.osg33.el7
glite-lbjp-common-gss-3.1.3-2.2.osg33.el7
glite-lbjp-common-gss-debuginfo-3.1.3-2.2.osg33.el7
glite-lbjp-common-gss-devel-3.1.3-2.2.osg33.el7
globus-authz-3.10-1.osg33.el7
globus-authz-callout-error-3.5-1.osg33.el7
globus-authz-callout-error-debuginfo-3.5-1.osg33.el7
globus-authz-callout-error-devel-3.5-1.osg33.el7
globus-authz-callout-error-doc-3.5-1.osg33.el7
globus-authz-debuginfo-3.10-1.osg33.el7
globus-authz-devel-3.10-1.osg33.el7
globus-authz-doc-3.10-1.osg33.el7
globus-callout-3.13-1.osg33.el7
globus-callout-debuginfo-3.13-1.osg33.el7
globus-callout-devel-3.13-1.osg33.el7
globus-callout-doc-3.13-1.osg33.el7
globus-common-15.27-1.osg33.el7
globus-common-debuginfo-15.27-1.osg33.el7
globus-common-devel-15.27-1.osg33.el7
globus-common-doc-15.27-1.osg33.el7
globus-common-progs-15.27-1.osg33.el7
globus-ftp-client-8.19-1.2.osg33.el7
globus-ftp-client-debuginfo-8.19-1.2.osg33.el7
globus-ftp-client-devel-8.19-1.2.osg33.el7
globus-ftp-client-doc-8.19-1.2.osg33.el7
globus-ftp-control-6.6-1.1.osg33.el7
globus-ftp-control-debuginfo-6.6-1.1.osg33.el7
globus-ftp-control-devel-6.6-1.1.osg33.el7
globus-ftp-control-doc-6.6-1.1.osg33.el7
globus-gass-cache-9.5-1.osg33.el7
globus-gass-cache-debuginfo-9.5-1.osg33.el7
globus-gass-cache-devel-9.5-1.osg33.el7
globus-gass-cache-doc-9.5-1.osg33.el7
globus-gass-cache-program-6.5-1.osg33.el7
globus-gass-cache-program-debuginfo-6.5-1.osg33.el7
globus-gass-copy-9.13-1.osg33.el7
globus-gass-copy-debuginfo-9.13-1.osg33.el7
globus-gass-copy-devel-9.13-1.osg33.el7
globus-gass-copy-doc-9.13-1.osg33.el7
globus-gass-copy-progs-9.13-1.osg33.el7
globus-gass-server-ez-5.7-1.osg33.el7
globus-gass-server-ez-debuginfo-5.7-1.osg33.el7
globus-gass-server-ez-devel-5.7-1.osg33.el7
globus-gass-server-ez-progs-5.7-1.osg33.el7
globus-gass-transfer-8.8-1.osg33.el7
globus-gass-transfer-debuginfo-8.8-1.osg33.el7
globus-gass-transfer-devel-8.8-1.osg33.el7
globus-gass-transfer-doc-8.8-1.osg33.el7
globus-gatekeeper-10.9-1.2.osg33.el7
globus-gatekeeper-debuginfo-10.9-1.2.osg33.el7
globus-gfork-4.7-1.osg33.el7
globus-gfork-debuginfo-4.7-1.osg33.el7
globus-gfork-devel-4.7-1.osg33.el7
globus-gfork-progs-4.7-1.osg33.el7
globus-gram-audit-4.4-1.osg33.el7
globus-gram-client-13.12-1.osg33.el7
globus-gram-client-debuginfo-13.12-1.osg33.el7
globus-gram-client-devel-13.12-1.osg33.el7
globus-gram-client-doc-13.12-1.osg33.el7
globus-gram-client-tools-11.7-1.osg33.el7
globus-gram-client-tools-debuginfo-11.7-1.osg33.el7
globus-gram-job-manager-14.25-1.2.osg33.el7
globus-gram-job-manager-callout-error-3.5-1.osg33.el7
globus-gram-job-manager-callout-error-debuginfo-3.5-1.osg33.el7
globus-gram-job-manager-callout-error-devel-3.5-1.osg33.el7
globus-gram-job-manager-callout-error-doc-3.5-1.osg33.el7
globus-gram-job-manager-condor-2.5-1.1.osg33.el7
globus-gram-job-manager-debuginfo-14.25-1.2.osg33.el7
globus-gram-job-manager-fork-2.4-1.1.osg33.el7
globus-gram-job-manager-fork-debuginfo-2.4-1.1.osg33.el7
globus-gram-job-manager-fork-setup-poll-2.4-1.1.osg33.el7
globus-gram-job-manager-fork-setup-seg-2.4-1.1.osg33.el7
globus-gram-job-manager-lsf-2.6-1.2.osg33.el7
globus-gram-job-manager-lsf-debuginfo-2.6-1.2.osg33.el7
globus-gram-job-manager-lsf-setup-poll-2.6-1.2.osg33.el7
globus-gram-job-manager-lsf-setup-seg-2.6-1.2.osg33.el7
globus-gram-job-manager-managedfork-0.2-1.osg33.el7
globus-gram-job-manager-pbs-2.4-2.1.osg33.el7
globus-gram-job-manager-pbs-debuginfo-2.4-2.1.osg33.el7
globus-gram-job-manager-pbs-setup-poll-2.4-2.1.osg33.el7
globus-gram-job-manager-pbs-setup-seg-2.4-2.1.osg33.el7
globus-gram-job-manager-scripts-6.7-1.osg33.el7
globus-gram-job-manager-scripts-doc-6.7-1.osg33.el7
globus-gram-job-manager-sge-2.5-1.1.osg33.el7
globus-gram-job-manager-sge-debuginfo-2.5-1.1.osg33.el7
globus-gram-job-manager-sge-setup-poll-2.5-1.1.osg33.el7
globus-gram-job-manager-sge-setup-seg-2.5-1.1.osg33.el7
globus-gram-protocol-12.12-2.osg33.el7
globus-gram-protocol-debuginfo-12.12-2.osg33.el7
globus-gram-protocol-devel-12.12-2.osg33.el7
globus-gram-protocol-doc-12.12-2.osg33.el7
globus-gridftp-server-7.20-1.1.osg33.el7
globus-gridftp-server-control-3.6-1.osg33.el7
globus-gridftp-server-control-debuginfo-3.6-1.osg33.el7
globus-gridftp-server-control-devel-3.6-1.osg33.el7
globus-gridftp-server-debuginfo-7.20-1.1.osg33.el7
globus-gridftp-server-devel-7.20-1.1.osg33.el7
globus-gridftp-server-progs-7.20-1.1.osg33.el7
globus-gridmap-callout-error-2.4-1.osg33.el7
globus-gridmap-callout-error-debuginfo-2.4-1.osg33.el7
globus-gridmap-callout-error-devel-2.4-1.osg33.el7
globus-gridmap-callout-error-doc-2.4-1.osg33.el7
globus-gsi-callback-5.7-1.osg33.el7
globus-gsi-callback-debuginfo-5.7-1.osg33.el7
globus-gsi-callback-devel-5.7-1.osg33.el7
globus-gsi-callback-doc-5.7-1.osg33.el7
globus-gsi-cert-utils-9.10-1.osg33.el7
globus-gsi-cert-utils-debuginfo-9.10-1.osg33.el7
globus-gsi-cert-utils-devel-9.10-1.osg33.el7
globus-gsi-cert-utils-doc-9.10-1.osg33.el7
globus-gsi-cert-utils-progs-9.10-1.osg33.el7
globus-gsi-credential-7.7-1.osg33.el7
globus-gsi-credential-debuginfo-7.7-1.osg33.el7
globus-gsi-credential-devel-7.7-1.osg33.el7
globus-gsi-credential-doc-7.7-1.osg33.el7
globus-gsi-openssl-error-3.5-1.osg33.el7
globus-gsi-openssl-error-debuginfo-3.5-1.osg33.el7
globus-gsi-openssl-error-devel-3.5-1.osg33.el7
globus-gsi-openssl-error-doc-3.5-1.osg33.el7
globus-gsi-proxy-core-7.7-1.osg33.el7
globus-gsi-proxy-core-debuginfo-7.7-1.osg33.el7
globus-gsi-proxy-core-devel-7.7-1.osg33.el7
globus-gsi-proxy-core-doc-7.7-1.osg33.el7
globus-gsi-proxy-ssl-5.7-1.osg33.el7
globus-gsi-proxy-ssl-debuginfo-5.7-1.osg33.el7
globus-gsi-proxy-ssl-devel-5.7-1.osg33.el7
globus-gsi-proxy-ssl-doc-5.7-1.osg33.el7
globus-gsi-sysconfig-6.8-1.osg33.el7
globus-gsi-sysconfig-debuginfo-6.8-1.osg33.el7
globus-gsi-sysconfig-devel-6.8-1.osg33.el7
globus-gsi-sysconfig-doc-6.8-1.osg33.el7
globus-gssapi-error-5.4-1.osg33.el7
globus-gssapi-error-debuginfo-5.4-1.osg33.el7
globus-gssapi-error-devel-5.4-1.osg33.el7
globus-gssapi-error-doc-5.4-1.osg33.el7
globus-gssapi-gsi-11.18-1.osg33.el7
globus-gssapi-gsi-debuginfo-11.18-1.osg33.el7
globus-gssapi-gsi-devel-11.18-1.osg33.el7
globus-gssapi-gsi-doc-11.18-1.osg33.el7
globus-gss-assist-10.13-1.osg33.el7
globus-gss-assist-debuginfo-10.13-1.osg33.el7
globus-gss-assist-devel-10.13-1.osg33.el7
globus-gss-assist-doc-10.13-1.osg33.el7
globus-gss-assist-progs-10.13-1.osg33.el7
globus-io-11.4-1.osg33.el7
globus-io-debuginfo-11.4-1.osg33.el7
globus-io-devel-11.4-1.osg33.el7
globus-openssl-module-4.6-1.osg33.el7
globus-openssl-module-debuginfo-4.6-1.osg33.el7
globus-openssl-module-devel-4.6-1.osg33.el7
globus-openssl-module-doc-4.6-1.osg33.el7
globus-proxy-utils-6.9-1.osg33.el7
globus-proxy-utils-debuginfo-6.9-1.osg33.el7
globus-rsl-10.9-1.osg33.el7
globus-rsl-debuginfo-10.9-1.osg33.el7
globus-rsl-devel-10.9-1.osg33.el7
globus-rsl-doc-10.9-1.osg33.el7
globus-scheduler-event-generator-5.10-2.1.osg33.el7
globus-scheduler-event-generator-debuginfo-5.10-2.1.osg33.el7
globus-scheduler-event-generator-devel-5.10-2.1.osg33.el7
globus-scheduler-event-generator-doc-5.10-2.1.osg33.el7
globus-scheduler-event-generator-progs-5.10-2.1.osg33.el7
globus-simple-ca-4.18-1.osg33.el7
globus-usage-4.4-1.osg33.el7
globus-usage-debuginfo-4.4-1.osg33.el7
globus-usage-devel-4.4-1.osg33.el7
globus-xio-5.7-1.1.osg33.el7
globus-xio-debuginfo-5.7-1.1.osg33.el7
globus-xio-devel-5.7-1.1.osg33.el7
globus-xio-doc-5.7-1.1.osg33.el7
globus-xio-gsi-driver-3.7-1.osg33.el7
globus-xio-gsi-driver-debuginfo-3.7-1.osg33.el7
globus-xio-gsi-driver-devel-3.7-1.osg33.el7
globus-xio-gsi-driver-doc-3.7-1.osg33.el7
globus-xioperf-4.4-1.osg33.el7
globus-xioperf-debuginfo-4.4-1.osg33.el7
globus-xio-pipe-driver-3.7-1.osg33.el7
globus-xio-pipe-driver-debuginfo-3.7-1.osg33.el7
globus-xio-pipe-driver-devel-3.7-1.osg33.el7
globus-xio-popen-driver-3.5-1.osg33.el7
globus-xio-popen-driver-debuginfo-3.5-1.osg33.el7
globus-xio-popen-driver-devel-3.5-1.osg33.el7
globus-xio-udt-driver-1.16-1.osg33.el7
globus-xio-udt-driver-debuginfo-1.16-1.osg33.el7
globus-xio-udt-driver-devel-1.16-1.osg33.el7
gratia-1.16.2-1.osg33.el7
gratia-debuginfo-1.16.2-1.osg33.el7
gratia-probe-1.14.2-6.osg33.el7
gratia-probe-bdii-status-1.14.2-6.osg33.el7
gratia-probe-common-1.14.2-6.osg33.el7
gratia-probe-condor-1.14.2-6.osg33.el7
gratia-probe-condor-events-1.14.2-6.osg33.el7
gratia-probe-dcache-storage-1.14.2-6.osg33.el7
gratia-probe-dcache-storagegroup-1.14.2-6.osg33.el7
gratia-probe-dcache-transfer-1.14.2-6.osg33.el7
gratia-probe-debuginfo-1.14.2-6.osg33.el7
gratia-probe-enstore-storage-1.14.2-6.osg33.el7
gratia-probe-enstore-tapedrive-1.14.2-6.osg33.el7
gratia-probe-enstore-transfer-1.14.2-6.osg33.el7
gratia-probe-glexec-1.14.2-6.osg33.el7
gratia-probe-glideinwms-1.14.2-6.osg33.el7
gratia-probe-gram-1.14.2-6.osg33.el7
gratia-probe-gridftp-transfer-1.14.2-6.osg33.el7
gratia-probe-hadoop-storage-1.14.2-6.osg33.el7
gratia-probe-lsf-1.14.2-6.osg33.el7
gratia-probe-metric-1.14.2-6.osg33.el7
gratia-probe-onevm-1.14.2-6.osg33.el7
gratia-probe-pbs-lsf-1.14.2-6.osg33.el7
gratia-probe-psacct-1.14.2-6.osg33.el7
gratia-probe-services-1.14.2-6.osg33.el7
gratia-probe-sge-1.14.2-6.osg33.el7
gratia-probe-slurm-1.14.2-6.osg33.el7
gratia-probe-xrootd-storage-1.14.2-6.osg33.el7
gratia-probe-xrootd-transfer-1.14.2-6.osg33.el7
gratia-reporting-email-1.15.1-1.osg33.el7
gratia-service-1.16.2-1.osg33.el7
gsi-openssh-5.7-1.1.osg33.el7
gsi-openssh-clients-5.7-1.1.osg33.el7
gsi-openssh-debuginfo-5.7-1.1.osg33.el7
gsi-openssh-server-5.7-1.1.osg33.el7
htcondor-ce-1.14-4.osg33.el7
htcondor-ce-client-1.14-4.osg33.el7
htcondor-ce-collector-1.14-4.osg33.el7
htcondor-ce-condor-1.14-4.osg33.el7
htcondor-ce-debuginfo-1.14-4.osg33.el7
htcondor-ce-lsf-1.14-4.osg33.el7
htcondor-ce-pbs-1.14-4.osg33.el7
htcondor-ce-sge-1.14-4.osg33.el7
I2util-1.1-2.osg33.el7
I2util-debuginfo-1.1-2.osg33.el7
igtf-ca-certs-1.65-1.osg33.el7
javascriptrrd-1.1.1-1.osg33.el7
jetty-8.1.4.v20120524-2.osg33.el7
jetty-ajp-8.1.4.v20120524-2.osg33.el7
jetty-annotations-8.1.4.v20120524-2.osg33.el7
jetty-client-8.1.4.v20120524-2.osg33.el7
jetty-continuation-8.1.4.v20120524-2.osg33.el7
jetty-deploy-8.1.4.v20120524-2.osg33.el7
jetty-http-8.1.4.v20120524-2.osg33.el7
jetty-io-8.1.4.v20120524-2.osg33.el7
jetty-jmx-8.1.4.v20120524-2.osg33.el7
jetty-jndi-8.1.4.v20120524-2.osg33.el7
jetty-overlay-deployer-8.1.4.v20120524-2.osg33.el7
jetty-plus-8.1.4.v20120524-2.osg33.el7
jetty-policy-8.1.4.v20120524-2.osg33.el7
jetty-rewrite-8.1.4.v20120524-2.osg33.el7
jetty-security-8.1.4.v20120524-2.osg33.el7
jetty-server-8.1.4.v20120524-2.osg33.el7
jetty-servlet-8.1.4.v20120524-2.osg33.el7
jetty-servlets-8.1.4.v20120524-2.osg33.el7
jetty-util-8.1.4.v20120524-2.osg33.el7
jetty-webapp-8.1.4.v20120524-2.osg33.el7
jetty-websocket-8.1.4.v20120524-2.osg33.el7
jetty-xml-8.1.4.v20120524-2.osg33.el7
joda-time-1.5.2-7.2.tzdata2008d.osg33.el7
joda-time-javadoc-1.5.2-7.2.tzdata2008d.osg33.el7
koji-1.6.0-10.osg33.el7
koji-builder-1.6.0-10.osg33.el7
koji-hub-1.6.0-10.osg33.el7
koji-hub-plugins-1.6.0-10.osg33.el7
koji-utils-1.6.0-10.osg33.el7
koji-vm-1.6.0-10.osg33.el7
koji-web-1.6.0-10.osg33.el7
lcas-lcmaps-gt4-interface-0.2.6-1.1.osg33.el7
lcas-lcmaps-gt4-interface-debuginfo-0.2.6-1.1.osg33.el7
lcmaps-1.6.6-1.1.osg33.el7
lcmaps-common-devel-1.6.6-1.1.osg33.el7
lcmaps-debuginfo-1.6.6-1.1.osg33.el7
lcmaps-devel-1.6.6-1.1.osg33.el7
lcmaps-plugins-basic-1.7.0-2.osg33.el7
lcmaps-plugins-basic-debuginfo-1.7.0-2.osg33.el7
lcmaps-plugins-basic-ldap-1.7.0-2.osg33.el7
lcmaps-plugins-glexec-tracking-0.1.6-1.osg33.el7
lcmaps-plugins-glexec-tracking-debuginfo-0.1.6-1.osg33.el7
lcmaps-plugins-gums-client-0.0.2-4.osg33.el7
lcmaps-plugins-scas-client-0.5.5-1.osg33.el7
lcmaps-plugins-scas-client-debuginfo-0.5.5-1.osg33.el7
lcmaps-plugins-verify-proxy-1.5.7-1.osg33.el7
lcmaps-plugins-verify-proxy-debuginfo-1.5.7-1.osg33.el7
lcmaps-without-gsi-1.6.6-1.1.osg33.el7
lcmaps-without-gsi-devel-1.6.6-1.1.osg33.el7
llrun-0.1.3-1.3.osg33.el7
llrun-debuginfo-0.1.3-1.3.osg33.el7
mash-0.5.22-3.osg33.el7
mkgltempdir-0.0.5-1.1.osg33.el7
myproxy-6.1.12-1.osg33.el7
myproxy-admin-6.1.12-1.osg33.el7
myproxy-debuginfo-6.1.12-1.osg33.el7
myproxy-devel-6.1.12-1.osg33.el7
myproxy-doc-6.1.12-1.osg33.el7
myproxy-libs-6.1.12-1.osg33.el7
myproxy-server-6.1.12-1.osg33.el7
myproxy-voms-6.1.12-1.osg33.el7
ndt-3.6.5-3.osg33.el7
ndt-client-3.6.5-3.osg33.el7
ndt-debuginfo-3.6.5-3.osg33.el7
ndt-server-3.6.5-3.osg33.el7
netlogger-4.2.0-9.osg33.el7
nuttcp-6.1.2-1.osg33.el7
nuttcp-debuginfo-6.1.2-1.osg33.el7
osg-base-ce-3.3-3_clipped.osg33.el7
osg-base-ce-condor-3.3-3_clipped.osg33.el7
osg-base-ce-lsf-3.3-3_clipped.osg33.el7
osg-base-ce-pbs-3.3-3_clipped.osg33.el7
osg-base-ce-sge-3.3-3_clipped.osg33.el7
osg-base-ce-slurm-3.3-3_clipped.osg33.el7
osg-build-1.6.0-2.osg33.el7
osg-ca-certs-1.47-1.osg33.el7
osg-ca-certs-updater-1.0-1.osg33.el7
osg-ca-scripts-1.1.5-2.osg33.el7
osg-ce-3.3-3_clipped.osg33.el7
osg-ce-condor-3.3-3_clipped.osg33.el7
osg-ce-lsf-3.3-3_clipped.osg33.el7
osg-ce-pbs-3.3-3_clipped.osg33.el7
osg-cert-scripts-2.7.2-2.osg33.el7
osg-ce-sge-3.3-3_clipped.osg33.el7
osg-ce-slurm-3.3-3_clipped.osg33.el7
osg-cleanup-1.7.2-1.osg33.el7
osg-configure-1.1.1-1.osg33.el7
osg-configure-ce-1.1.1-1.osg33.el7
osg-configure-cemon-1.1.1-1.osg33.el7
osg-configure-condor-1.1.1-1.osg33.el7
osg-configure-gateway-1.1.1-1.osg33.el7
osg-configure-gip-1.1.1-1.osg33.el7
osg-configure-gratia-1.1.1-1.osg33.el7
osg-configure-infoservices-1.1.1-1.osg33.el7
osg-configure-lsf-1.1.1-1.osg33.el7
osg-configure-managedfork-1.1.1-1.osg33.el7
osg-configure-misc-1.1.1-1.osg33.el7
osg-configure-monalisa-1.1.1-1.osg33.el7
osg-configure-network-1.1.1-1.osg33.el7
osg-configure-pbs-1.1.1-1.osg33.el7
osg-configure-rsv-1.1.1-1.osg33.el7
osg-configure-sge-1.1.1-1.osg33.el7
osg-configure-slurm-1.1.1-1.osg33.el7
osg-configure-squid-1.1.1-1.osg33.el7
osg-configure-tests-1.1.1-1.osg33.el7
osg-control-1.0.1-1.osg33.el7
osg-gridftp-3.3-2_clipped.osg33.el7
osg-gridftp-hdfs-3.3-2.osg33.el7
osg-gridftp-xrootd-3.3-2.osg33.el7
osg-gums-3.3-2.osg33.el7
osg-gums-config-61-1.osg33.el7
osg-htcondor-ce-3.3-3_clipped.osg33.el7
osg-htcondor-ce-condor-3.3-3_clipped.osg33.el7
osg-htcondor-ce-lsf-3.3-3_clipped.osg33.el7
osg-htcondor-ce-pbs-3.3-3_clipped.osg33.el7
osg-htcondor-ce-sge-3.3-3_clipped.osg33.el7
osg-htcondor-ce-slurm-3.3-3_clipped.osg33.el7
osg-info-services-1.0.2-1.osg33.el7
osg-java7-compat-1.0-1.osg33.el7
osg-java7-compat-openjdk-1.0-1.osg33.el7
osg-java7-devel-compat-1.0-1.osg33.el7
osg-java7-devel-compat-openjdk-1.0-1.osg33.el7
osg-oasis-5-2.osg33.el7
osg-pki-tools-1.2.12-1.osg33.el7
osg-pki-tools-tests-1.2.12-1.osg33.el7
osg-release-3.3-2.osg33.el7
osg-release-itb-3.3-2.osg33.el7
osg-se-bestman-3.3-2.osg33.el7
osg-se-bestman-xrootd-3.3-2.osg33.el7
osg-se-hadoop-3.3-2.osg33.el7
osg-se-hadoop-client-3.3-2.osg33.el7
osg-se-hadoop-datanode-3.3-2.osg33.el7
osg-se-hadoop-gridftp-3.3-2.osg33.el7
osg-se-hadoop-namenode-3.3-2.osg33.el7
osg-se-hadoop-secondarynamenode-3.3-2.osg33.el7
osg-se-hadoop-srm-3.3-2.osg33.el7
osg-system-profiler-1.2.0-1.osg33.el7
osg-system-profiler-viewer-1.2.0-1.osg33.el7
osg-test-1.4.25-1.osg33.el7
osg-tested-internal-3.3-1.osg33.el7
osg-version-3.3.0-1.osg33.el7
osg-vo-map-0.0.1-1.osg33.el7
osg-voms-3.3-1.osg33.el7
osg-webapp-common-1-2.osg33.el7
osg-wn-client-3.3-5.osg33.el7
osg-wn-client-glexec-3.3-5.osg33.el7
owamp-3.2rc4-2.osg33.el7
owamp-client-3.2rc4-2.osg33.el7
owamp-debuginfo-3.2rc4-2.osg33.el7
owamp-server-3.2rc4-2.osg33.el7
pegasus-4.3.1-1.3.osg33.el7
pegasus-debuginfo-4.3.1-1.3.osg33.el7
rsv-3.10.2-1_clipped.osg33.el7
rsv-consumers-3.10.2-1_clipped.osg33.el7
rsv-core-3.10.2-1_clipped.osg33.el7
rsv-metrics-3.10.2-1_clipped.osg33.el7
rsv-perfsonar-1.0.19-1.osg33.el7
rsv-vo-gwms-1.0.1-1.osg33.el7
stashcache-0.3-4.osg33.el7
stashcache-cache-server-0.3-4.osg33.el7
stashcache-daemon-0.2-1.osg33.el7
stashcache-daemon-0.3-4.osg33.el7
stashcache-origin-server-0.3-4.osg33.el7
uberftp-2.8-2.1.osg33.el7
uberftp-debuginfo-2.8-2.1.osg33.el7
vo-client-61-1.osg33.el7
vo-client-edgmkgridmap-61-1.osg33.el7
voms-2.0.12-3.osg33.el7
voms-admin-client-2.0.17-1.1.osg33.el7
voms-api-java-2.0.8-1.6.osg33.el7
voms-api-java-javadoc-2.0.8-1.6.osg33.el7
voms-clients-cpp-2.0.12-3.osg33.el7
voms-debuginfo-2.0.12-3.osg33.el7
voms-devel-2.0.12-3.osg33.el7
voms-doc-2.0.12-3.osg33.el7
voms-mysql-plugin-3.1.6-1.1.osg33.el7
voms-mysql-plugin-debuginfo-3.1.6-1.1.osg33.el7
voms-server-2.0.12-3.osg33.el7
web100_userland-1.7-6.osg33.el7
web100_userland-debuginfo-1.7-6.osg33.el7
xacml-1.5.0-1.osg33.el7
xacml-debuginfo-1.5.0-1.osg33.el7
xacml-devel-1.5.0-1.osg33.el7
xrootd-4.2.2-1.osg33.el7
xrootd-client-4.2.2-1.osg33.el7
xrootd-client-devel-4.2.2-1.osg33.el7
xrootd-client-libs-4.2.2-1.osg33.el7
xrootd-debuginfo-4.2.2-1.osg33.el7
xrootd-devel-4.2.2-1.osg33.el7
xrootd-doc-4.2.2-1.osg33.el7
xrootd-dsi-3.0.4-16.osg33.el7
xrootd-dsi-debuginfo-3.0.4-16.osg33.el7
xrootd-fuse-4.2.2-1.osg33.el7
xrootd-lcmaps-0.0.7-11.osg33.el7
xrootd-lcmaps-debuginfo-0.0.7-11.osg33.el7
xrootd-libs-4.2.2-1.osg33.el7
xrootd-private-devel-4.2.2-1.osg33.el7
xrootd-python-4.2.2-1.osg33.el7
xrootd-selinux-4.2.2-1.osg33.el7
xrootd-server-4.2.2-1.osg33.el7
xrootd-server-devel-4.2.2-1.osg33.el7
xrootd-server-libs-4.2.2-1.osg33.el7
xrootd-status-probe-0.0.3-11.osg33.el7
xrootd-status-probe-debuginfo-0.0.3-11.osg33.el7
xrootd-voms-plugin-0.2.0-1.6.osg33.el7
xrootd-voms-plugin-debuginfo-0.2.0-1.6.osg33.el7
xrootd-voms-plugin-devel-0.2.0-1.6.osg33.el7
```

### Upcoming Packages

We added or updated the following packages to the **upcoming** OSG yum repository. Note that in some cases, there are multiple RPMs for each package. You can click on any given package to see the set of RPMs or see the complete list below.

#### Enterprise Linux 5

#### Enterprise Linux 6

### Upcoming RPMs

If you wish to manually update your system, you can run yum update against the following packages:

If you wish to only update the RPMs that changed, the set of RPMs is:

#### Enterprise Linux 5

``` file
```

#### Enterprise Linux 6

``` file
```

OSG Software Release 3.3.1
==========================

**Release Date**: 2015-09-08

Summary of changes
------------------

This release contains:

-   Enterprise Linux 7: Worker node support only
-   GUMS 1.5.0
    -   A security-related bug was found in the GUMS administrative interface. The risk of this vulnerability has been assessed by OSG Security Team as “Critical”. A new GUMS version 1.5.0 has been released and it is included in OSG Software version 3.2.27/3.3.1. Site administrators should update ASAP.
-   [HTCondor 8.3.8](https://www-auth.cs.wisc.edu/lists/htcondor-users/2015-August/msg00153.shtml)
-   HTCondor CE 1.15
    -   Add 'default\_remote\_cerequirements' attribute to the JOB\_ROUTER\_DEFAULTS
    -   Verify the first route in JOB\_ROUTER\_ENTRIES in the init script
    -   htcondor-ce-collecotr now uses /etc/sysconfig/condor-ce-collector for additional configuration
-   gridFTP-HDFS checksum verification improvements
    -   Checksum algorithm names are checked with a case insensitive comparison
    -   Error codes are properly returned to the user
-   CA certificates based on [IGTF 1.67](https://dist.eugridpma.info/distribution/igtf/current/CHANGES)
-   StashCache improvements
    -   Added logging to the StashCache daemon
    -   Advertise the StashCache daemon version to the master ClassAd
    -   Improved example configuration files to include "pss.trace all"

These [JIRA tickets](https://jira.opensciencegrid.org/issues/?jql=project%20%3D%20SOFTWARE%20AND%20fixVersion%20%3D%203.3.1%20ORDER%20BY%20priority%20DESC) were addressed in this release.

Detailed changes are below. All of the documentation can be found in the [Release3](https://twiki.grid.iu.edu/bin/view/Documentation/Release3/) area of the TWiki.

Known Issues
------------

-   The HTCondor shared port daemon can run out of file descriptors.
    -   This is a problem with HTCondor 8.3.7 and 8.3.8.
    -   The problem will be fixed in the HTCondor 8.4.0 release.
    -   The glideInWMS factories (and possibly front-ends) using the following configuration model will cause the HTCondor shared port daemon to leak file descriptors. The relevant configuration fragment is similar to:

            :::file
            USE_SHARED_PORT = FALSE
            DAEMON_LIST = $(DAEMON_LIST) SHARED_PORT
            SCHEDD.USE_SHARED_PORT = TRUE
            SHADOW.USE_SHADED_PORT = TRUE

-   This non-standard shared-port configuration is probably used to avoid dealing with shared-port collector trees.
-   The work-around is to start the HTCondor master with CONDOR\_PRIVATE\_SHARED\_PORT\_COOKIE set in the environment, as it will propagate down to the target daemons. CONDOR\_PRIVATE\_SHARED\_PORT\_COOKIE should contain a 32-byte random number in hexadecimal format (64 characters). **Note:** care must be taken to ensure that this value is private. Upgrade to HTCondor 8.4.0 as soon as possible and stop using this workaround. \* Running osg-configure with the new version of HTCondor installed causes a deprecation warning to be emitted. The warning does not affect the services. \* StashCache packages need to be manually configured
-   Manual configuration for origin server
    -   Assuming that the origin server connects only to a redirector (not directly to cache server), minimal xrootd configuration is required. The configuration file, /etc/xrootd/xrootd-stashcache-origin-server.cfg, in this release is overkill. Here are recommended settings to use:

            :::file
            xrd.port 1094
            all.role server
            all.manager stash-redirector.example.com 1213
            all.export / nostage
            xrootd.trace emsg login stall redirect
            ofs.trace none
            xrd.trace conn
            cms.trace all
            sec.protocol  host
            sec.protbind  * none
            all.adminpath /var/run/xrootd
            all.pidpath /var/run/xrootd

-   Manual configuration for cache server
    -   In contrast to the origin server configuration, one needs to declare `pss.origin <stash-redirector.example.com>` instead of configuring the cmsd or manager (only the xrootd daemon is required on the cache server). More detailed configuration of cache server for StashCache is [here](https://confluence.grid.iu.edu/pages/viewpage.action?title=Installing+an+XRootD+server+for+Stash+Cache&spaceKey=STAS).
-   In both cases, administrator needs to set the path of custom configuration file for its xrootd/cmds instance in /etc/sysconfig/xrootd, For example, change the cmds default from:

        :::file
        CMSD_DEFAULT_OPTIONS="-l /var/log/xrootd/cmsd.log -c /etc/xrootd/xrootd-clustered.cfg -k fifo"
<br/>

    to

        :::file
        CMSD_DEFAULT_OPTIONS="-l /var/log/xrootd/cmsd.log -c /etc/xrootd/xrootd-stashcache-origin-server.marian -k fifo" 

Updating to the new release
---------------------------

### Update Repositories

To update to this series, you need [install the current OSG repositories](../../common/yum#install-osg-repositories).

### Update Software

Once the new repositories are installed, you can update to this new release with:

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

#### Enterprise Linux 6

-   [blahp-1.18.13.bosco-4.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=blahp-1.18.13.bosco-4.osg33.el6)
-   [condor-8.3.8-1.1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=condor-8.3.8-1.1.osg33.el6)
-   [emi-trustmanager-tomcat-3.0.0-12.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=emi-trustmanager-tomcat-3.0.0-12.osg33.el6)
-   [gridftp-hdfs-0.5.4-20.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=gridftp-hdfs-0.5.4-20.osg33.el6)
-   [gums-1.5.0-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=gums-1.5.0-1.osg33.el6)
-   [hadoop-2.0.0+545-1.cdh4.1.1.p0.21.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=hadoop-2.0.0+545-1.cdh4.1.1.p0.21.osg33.el6)
-   [htcondor-ce-1.15-2.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=htcondor-ce-1.15-2.osg33.el6)
-   [igtf-ca-certs-1.67-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=igtf-ca-certs-1.67-1.osg33.el6)
-   [osg-build-1.6.1-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-build-1.6.1-1.osg33.el6)
-   [osg-ca-certs-1.48-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-ca-certs-1.48-1.osg33.el6)
-   [osg-test-1.4.29-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-test-1.4.29-1.osg33.el6)
-   [osg-tested-internal-3.3-3.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-tested-internal-3.3-3.osg33.el6)
-   [osg-version-3.3.1-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-version-3.3.1-1.osg33.el6)
-   [rsv-3.10.3-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=rsv-3.10.3-1.osg33.el6)
-   [stashcache-0.4-2.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=stashcache-0.4-2.osg33.el6)

#### Enterprise Linux 7

-   [blahp-1.18.13.bosco-4.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=blahp-1.18.13.bosco-4.osg33.el7)
-   [condor-8.3.8-1.1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=condor-8.3.8-1.1.osg33.el7)
-   [emi-trustmanager-tomcat-3.0.0-12.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=emi-trustmanager-tomcat-3.0.0-12.osg33.el7)
-   [htcondor-ce-1.15-2.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=htcondor-ce-1.15-2.osg33.el7)
-   [igtf-ca-certs-1.67-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=igtf-ca-certs-1.67-1.osg33.el7)
-   [osg-build-1.6.1-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-build-1.6.1-1.osg33.el7)
-   [osg-ca-certs-1.48-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-ca-certs-1.48-1.osg33.el7)
-   [osg-test-1.4.29-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-test-1.4.29-1.osg33.el7)
-   [osg-tested-internal-3.3-3.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-tested-internal-3.3-3.osg33.el7)
-   [osg-version-3.3.1-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-version-3.3.1-1.osg33.el7)
-   [rsv-3.10.3-1\_clipped.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=rsv-3.10.3-1_clipped.osg33.el7)
-   [stashcache-0.4-2.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=stashcache-0.4-2.osg33.el7)

### RPMs

If you wish to manually update your system, you can run yum update against the following packages:

    blahp blahp-debuginfo condor condor-all condor-bosco condor-classads condor-classads-devel condor-cream-gahp condor-debuginfo condor-kbdd condor-procd condor-python condor-std-universe condor-test condor-vm-gahp emi-trustmanager-tomcat gridftp-hdfs gridftp-hdfs-debuginfo gums gums-client gums-service hadoop hadoop-client hadoop-conf-pseudo hadoop-debuginfo hadoop-doc hadoop-hdfs hadoop-hdfs-datanode hadoop-hdfs-fuse hadoop-hdfs-fuse-selinux hadoop-hdfs-journalnode hadoop-hdfs-namenode hadoop-hdfs-secondarynamenode hadoop-hdfs-zkfc hadoop-httpfs hadoop-libhdfs hadoop-mapreduce hadoop-yarn htcondor-ce htcondor-ce-client htcondor-ce-collector htcondor-ce-condor htcondor-ce-debuginfo htcondor-ce-lsf htcondor-ce-pbs htcondor-ce-sge igtf-ca-certs osg-build osg-ca-certs osg-test osg-tested-internal osg-version python-six rsv rsv-consumers rsv-core rsv-metrics stashcache-cache-server stashcache-daemon stashcache-origin-server

If you wish to only update the RPMs that changed, the set of RPMs is:

#### Enterprise Linux 6

``` file
blahp-1.18.13.bosco-4.osg33.el6
blahp-debuginfo-1.18.13.bosco-4.osg33.el6
condor-8.3.8-1.1.osg33.el6
condor-all-8.3.8-1.1.osg33.el6
condor-bosco-8.3.8-1.1.osg33.el6
condor-classads-8.3.8-1.1.osg33.el6
condor-classads-devel-8.3.8-1.1.osg33.el6
condor-cream-gahp-8.3.8-1.1.osg33.el6
condor-debuginfo-8.3.8-1.1.osg33.el6
condor-kbdd-8.3.8-1.1.osg33.el6
condor-procd-8.3.8-1.1.osg33.el6
condor-python-8.3.8-1.1.osg33.el6
condor-std-universe-8.3.8-1.1.osg33.el6
condor-test-8.3.8-1.1.osg33.el6
condor-vm-gahp-8.3.8-1.1.osg33.el6
emi-trustmanager-tomcat-3.0.0-12.osg33.el6
gridftp-hdfs-0.5.4-20.osg33.el6
gridftp-hdfs-debuginfo-0.5.4-20.osg33.el6
gums-1.5.0-1.osg33.el6
gums-client-1.5.0-1.osg33.el6
gums-service-1.5.0-1.osg33.el6
hadoop-2.0.0+545-1.cdh4.1.1.p0.21.osg33.el6
hadoop-client-2.0.0+545-1.cdh4.1.1.p0.21.osg33.el6
hadoop-conf-pseudo-2.0.0+545-1.cdh4.1.1.p0.21.osg33.el6
hadoop-debuginfo-2.0.0+545-1.cdh4.1.1.p0.21.osg33.el6
hadoop-doc-2.0.0+545-1.cdh4.1.1.p0.21.osg33.el6
hadoop-hdfs-2.0.0+545-1.cdh4.1.1.p0.21.osg33.el6
hadoop-hdfs-datanode-2.0.0+545-1.cdh4.1.1.p0.21.osg33.el6
hadoop-hdfs-fuse-2.0.0+545-1.cdh4.1.1.p0.21.osg33.el6
hadoop-hdfs-fuse-selinux-2.0.0+545-1.cdh4.1.1.p0.21.osg33.el6
hadoop-hdfs-journalnode-2.0.0+545-1.cdh4.1.1.p0.21.osg33.el6
hadoop-hdfs-namenode-2.0.0+545-1.cdh4.1.1.p0.21.osg33.el6
hadoop-hdfs-secondarynamenode-2.0.0+545-1.cdh4.1.1.p0.21.osg33.el6
hadoop-hdfs-zkfc-2.0.0+545-1.cdh4.1.1.p0.21.osg33.el6
hadoop-httpfs-2.0.0+545-1.cdh4.1.1.p0.21.osg33.el6
hadoop-libhdfs-2.0.0+545-1.cdh4.1.1.p0.21.osg33.el6
hadoop-mapreduce-2.0.0+545-1.cdh4.1.1.p0.21.osg33.el6
hadoop-yarn-2.0.0+545-1.cdh4.1.1.p0.21.osg33.el6
htcondor-ce-1.15-2.osg33.el6
htcondor-ce-client-1.15-2.osg33.el6
htcondor-ce-collector-1.15-2.osg33.el6
htcondor-ce-condor-1.15-2.osg33.el6
htcondor-ce-debuginfo-1.15-2.osg33.el6
htcondor-ce-lsf-1.15-2.osg33.el6
htcondor-ce-pbs-1.15-2.osg33.el6
htcondor-ce-sge-1.15-2.osg33.el6
igtf-ca-certs-1.67-1.osg33.el6
osg-build-1.6.1-1.osg33.el6
osg-ca-certs-1.48-1.osg33.el6
osg-test-1.4.29-1.osg33.el6
osg-tested-internal-3.3-3.osg33.el6
osg-version-3.3.1-1.osg33.el6
rsv-3.10.3-1.osg33.el6
rsv-consumers-3.10.3-1.osg33.el6
rsv-core-3.10.3-1.osg33.el6
rsv-metrics-3.10.3-1.osg33.el6
stashcache-0.4-2.osg33.el6
stashcache-cache-server-0.4-2.osg33.el6
stashcache-daemon-0.4-2.osg33.el6
stashcache-origin-server-0.4-2.osg33.el6
```

#### Enterprise Linux 7

``` file
blahp-1.18.13.bosco-4.osg33.el7
blahp-debuginfo-1.18.13.bosco-4.osg33.el7
condor-8.3.8-1.1.osg33.el7
condor-all-8.3.8-1.1.osg33.el7
condor-bosco-8.3.8-1.1.osg33.el7
condor-classads-8.3.8-1.1.osg33.el7
condor-classads-devel-8.3.8-1.1.osg33.el7
condor-debuginfo-8.3.8-1.1.osg33.el7
condor-kbdd-8.3.8-1.1.osg33.el7
condor-procd-8.3.8-1.1.osg33.el7
condor-python-8.3.8-1.1.osg33.el7
condor-test-8.3.8-1.1.osg33.el7
condor-vm-gahp-8.3.8-1.1.osg33.el7
emi-trustmanager-tomcat-3.0.0-12.osg33.el7
htcondor-ce-1.15-2.osg33.el7
htcondor-ce-client-1.15-2.osg33.el7
htcondor-ce-collector-1.15-2.osg33.el7
htcondor-ce-condor-1.15-2.osg33.el7
htcondor-ce-debuginfo-1.15-2.osg33.el7
htcondor-ce-lsf-1.15-2.osg33.el7
htcondor-ce-pbs-1.15-2.osg33.el7
htcondor-ce-sge-1.15-2.osg33.el7
igtf-ca-certs-1.67-1.osg33.el7
osg-build-1.6.1-1.osg33.el7
osg-ca-certs-1.48-1.osg33.el7
osg-test-1.4.29-1.osg33.el7
osg-tested-internal-3.3-3.osg33.el7
osg-version-3.3.1-1.osg33.el7
rsv-3.10.3-1_clipped.osg33.el7
rsv-consumers-3.10.3-1_clipped.osg33.el7
rsv-core-3.10.3-1_clipped.osg33.el7
rsv-metrics-3.10.3-1_clipped.osg33.el7
stashcache-0.4-2.osg33.el7
stashcache-cache-server-0.4-2.osg33.el7
stashcache-daemon-0.4-2.osg33.el7
stashcache-origin-server-0.4-2.osg33.el7
```

### Upcoming Packages

We added or updated the following packages to the **upcoming** OSG yum repository. Note that in some cases, there are multiple RPMs for each package. You can click on any given package to see the set of RPMs or see the complete list below.

#### Enterprise Linux 5

#### Enterprise Linux 6

### Upcoming RPMs

If you wish to manually update your system, you can run yum update against the following packages:

If you wish to only update the RPMs that changed, the set of RPMs is:

#### Enterprise Linux 5

``` file
```

#### Enterprise Linux 6

``` file
```

OSG Software Release 3.3.2
==========================

**Release Date**: 2015-10-13

Summary of changes
------------------

This release contains:

-   [GlideinWMS 3.2.11.2](http://www.uscms.org/SoftwareComputing/Grid/WMS/glideinWMS/doc.prd/history.html)
-   [XRootD 4.2.3](https://github.com/xrootd/xrootd/blob/v4.2.3/docs/ReleaseNotes.txt)
-   [HTCondor 8.4.0](https://www-auth.cs.wisc.edu/lists/htcondor-users/2015-September/msg00029.shtml)
-   StashCache 0.6
    -   daemon refuses to start if host certificate is not present
    -   use FQDN in stashcache-daemon
-   GUMS 1.5.1
    -   Return groupName for pool account mappers
    -   Bug fixes
-   HTCondor-CE 1.16
    -   Updates for PBS variants
    -   Add CERN host DN format to HTCondor-CE configuration defaults
-   GIP support multiple SLURM queues
-   osg-configure 1.2.2
    -   Support IPv6 IP addresses in configuration files
    -   Add sensible default values for Allowed VOs
-   RSV - srmcp-srm-probe (delays to account for NFS caching behavior)
-   Add lcmaps-plugins-mount-under-scratch package
-   Update to edg-mkgridmap 4.0.3, so it works on EL 7
-   CA certificates based on [IGTF 1.68](https://dist.eugridpma.info/distribution/igtf/current/CHANGES)

These [JIRA tickets](https://jira.opensciencegrid.org/issues/?jql=project%20%3D%20SOFTWARE%20AND%20fixVersion%20%3D%203.3.2%20ORDER%20BY%20priority%20DESC) were addressed in this release.

Detailed changes are below. All of the documentation can be found in the [Release3](https://twiki.grid.iu.edu/bin/view/Documentation/Release3/) area of the TWiki.

Known Issues
------------

-   HTCondor 8.4.0 has changed it's behavior in ways that cause the GlideinWMS frontend configuration to break. In order to correct this, the following setting needs to be added to the configuration file:

        :::file
        COLLECTOR_USES_SHARED_PORT = False

-   StashCache packages need to be manually configured
    -   Manual configuration for origin server
        -   Assuming that the origin server connects only to a redirector (not directly to cache server), minimal xrootd configuration is required. The configuration file, /etc/xrootd/xrootd-stashcache-origin-server.cfg, in this release is overkill. Here are recommended settings to use:

                :::file
                xrd.port 1094
                all.role server
                all.manager stash-redirector.example.com 1213
                all.export / nostage
                xrootd.trace emsg login stall redirect
                ofs.trace none
                xrd.trace conn
                cms.trace all
                sec.protocol  host
                sec.protbind  * none
                all.adminpath /var/run/xrootd
                all.pidpath /var/run/xrootd

-   Manual configuration for cache server
    -   In contrast to the origin server configuration, one needs to declare `pss.origin <stash-redirector.example.com>` instead of configuring the cmsd or manager (only the xrootd daemon is required on the cache server). More detailed configuration of cache server for StashCache is [here](https://confluence.grid.iu.edu/pages/viewpage.action?title=Installing+an+XRootD+server+for+Stash+Cache&spaceKey=STAS).
-   In both cases, administrator needs to set the path of custom configuration file for its xrootd/cmds instance in /etc/sysconfig/xrootd, For example, change the cmds default from:

        :::file
        CMSD_DEFAULT_OPTIONS="-l /var/log/xrootd/cmsd.log -c /etc/xrootd/xrootd-clustered.cfg -k fifo"
<br/>

    to

        :::file
        CMSD_DEFAULT_OPTIONS="-l /var/log/xrootd/cmsd.log -c /etc/xrootd/xrootd-stashcache-origin-server.marian -k fifo" 

Updating to the new release
---------------------------

### Update Repositories

To update to this series, you need [install the current OSG repositories](../../common/yum#install-osg-repositories).

### Update Software

Once the new repositories are installed, you can update to this new release with:

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

#### Enterprise Linux 6

-   [bestman2-2.3.0-26.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=bestman2-2.3.0-26.osg33.el6)
-   [blahp-1.18.14.bosco-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=blahp-1.18.14.bosco-1.osg33.el6)
-   [condor-8.4.0-1.2.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=condor-8.4.0-1.2.osg33.el6)
-   [edg-mkgridmap-4.0.3-2.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=edg-mkgridmap-4.0.3-2.osg33.el6)
-   [gip-1.3.11-7.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=gip-1.3.11-7.osg33.el6)
-   [glideinwms-3.2.11.2-4.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=glideinwms-3.2.11.2-4.osg33.el6)
-   [gridftp-hdfs-0.5.4-22.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=gridftp-hdfs-0.5.4-22.osg33.el6)
-   [gums-1.5.1-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=gums-1.5.1-1.osg33.el6)
-   [htcondor-ce-1.16-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=htcondor-ce-1.16-1.osg33.el6)
-   [igtf-ca-certs-1.68-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=igtf-ca-certs-1.68-1.osg33.el6)
-   [jglobus-2.1.0-5.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=jglobus-2.1.0-5.osg33.el6)
-   [lcmaps-plugins-mount-under-scratch-0.0.4-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=lcmaps-plugins-mount-under-scratch-0.0.4-1.osg33.el6)
-   [osg-ca-certs-1.49-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-ca-certs-1.49-1.osg33.el6)
-   [osg-configure-1.2.2-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-configure-1.2.2-1.osg33.el6)
-   [osg-test-1.4.30-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-test-1.4.30-1.osg33.el6)
-   [osg-tested-internal-3.3-5.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-tested-internal-3.3-5.osg33.el6)
-   [osg-version-3.3.2-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-version-3.3.2-1.osg33.el6)
-   [privilege-xacml-2.6.5-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=privilege-xacml-2.6.5-1.osg33.el6)
-   [rsv-3.10.4-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=rsv-3.10.4-1.osg33.el6)
-   [stashcache-0.6-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=stashcache-0.6-1.osg33.el6)
-   [xrootd-4.2.3-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=xrootd-4.2.3-1.osg33.el6)

#### Enterprise Linux 7

-   [axis-1.4-23.1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=axis-1.4-23.1.osg33.el7)
-   [blahp-1.18.14.bosco-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=blahp-1.18.14.bosco-1.osg33.el7)
-   [condor-8.4.0-1.2.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=condor-8.4.0-1.2.osg33.el7)
-   [edg-mkgridmap-4.0.3-2.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=edg-mkgridmap-4.0.3-2.osg33.el7)
-   [gip-1.3.11-7.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=gip-1.3.11-7.osg33.el7)
-   [glideinwms-3.2.11.2-4.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=glideinwms-3.2.11.2-4.osg33.el7)
-   [htcondor-ce-1.16-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=htcondor-ce-1.16-1.osg33.el7)
-   [igtf-ca-certs-1.68-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=igtf-ca-certs-1.68-1.osg33.el7)
-   [javamail-1.5.0-6.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=javamail-1.5.0-6.osg33.el7)
-   [lcmaps-plugins-mount-under-scratch-0.0.4-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=lcmaps-plugins-mount-under-scratch-0.0.4-1.osg33.el7)
-   [osg-ca-certs-1.49-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-ca-certs-1.49-1.osg33.el7)
-   [osg-configure-1.2.2-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-configure-1.2.2-1.osg33.el7)
-   [osg-test-1.4.30-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-test-1.4.30-1.osg33.el7)
-   [osg-tested-internal-3.3-5.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-tested-internal-3.3-5.osg33.el7)
-   [osg-version-3.3.2-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-version-3.3.2-1.osg33.el7)
-   [rsv-3.10.4-1\_clipped.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=rsv-3.10.4-1_clipped.osg33.el7)
-   [stashcache-0.6-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=stashcache-0.6-1.osg33.el7)
-   [wsdl4j-1.6.3-3.1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=wsdl4j-1.6.3-3.1.osg33.el7)
-   [xrootd-4.2.3-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=xrootd-4.2.3-1.osg33.el7)

### RPMs

If you wish to manually update your system, you can run yum update against the following packages:

    bestman2-client bestman2-client-libs bestman2-common-libs bestman2-server bestman2-server-dep-libs bestman2-server-libs bestman2-tester bestman2-tester-libs blahp blahp-debuginfo condor condor-all condor-bosco condor-classads condor-classads-devel condor-cream-gahp condor-debuginfo condor-kbdd condor-procd condor-python condor-std-universe condor-test condor-vm-gahp edg-mkgridmap gip glideinwms-common-tools glideinwms-condor-common-config glideinwms-factory glideinwms-factory-condor glideinwms-glidecondor-tools glideinwms-libs glideinwms-minimal-condor glideinwms-usercollector glideinwms-userschedd glideinwms-vofrontend glideinwms-vofrontend-standalone gridftp-hdfs gridftp-hdfs-debuginfo gums gums-client gums-service htcondor-ce htcondor-ce-client htcondor-ce-collector htcondor-ce-condor htcondor-ce-debuginfo htcondor-ce-lsf htcondor-ce-pbs htcondor-ce-sge igtf-ca-certs jglobus lcmaps-plugins-mount-under-scratch lcmaps-plugins-mount-under-scratch-debuginfo osg-ca-certs osg-configure osg-configure-ce osg-configure-cemon osg-configure-condor osg-configure-gateway osg-configure-gip osg-configure-gratia osg-configure-infoservices osg-configure-lsf osg-configure-managedfork osg-configure-misc osg-configure-monalisa osg-configure-network osg-configure-pbs osg-configure-rsv osg-configure-sge osg-configure-slurm osg-configure-squid osg-configure-tests osg-test osg-tested-internal osg-version privilege-xacml python-argparse python-backports-ssl_match_hostname python-requests python-urllib3 rsv rsv-consumers rsv-core rsv-metrics stashcache-cache-server stashcache-daemon stashcache-origin-server xrootd xrootd-client xrootd-client-devel xrootd-client-libs xrootd-debuginfo xrootd-devel xrootd-doc xrootd-fuse xrootd-libs xrootd-private-devel xrootd-python xrootd-selinux xrootd-server xrootd-server-devel xrootd-server-libs

If you wish to only update the RPMs that changed, the set of RPMs is:

#### Enterprise Linux 6

``` file
bestman2-2.3.0-26.osg33.el6
bestman2-client-2.3.0-26.osg33.el6
bestman2-client-libs-2.3.0-26.osg33.el6
bestman2-common-libs-2.3.0-26.osg33.el6
bestman2-server-2.3.0-26.osg33.el6
bestman2-server-dep-libs-2.3.0-26.osg33.el6
bestman2-server-libs-2.3.0-26.osg33.el6
bestman2-tester-2.3.0-26.osg33.el6
bestman2-tester-libs-2.3.0-26.osg33.el6
blahp-1.18.14.bosco-1.osg33.el6
blahp-debuginfo-1.18.14.bosco-1.osg33.el6
condor-8.4.0-1.2.osg33.el6
condor-all-8.4.0-1.2.osg33.el6
condor-bosco-8.4.0-1.2.osg33.el6
condor-classads-8.4.0-1.2.osg33.el6
condor-classads-devel-8.4.0-1.2.osg33.el6
condor-cream-gahp-8.4.0-1.2.osg33.el6
condor-debuginfo-8.4.0-1.2.osg33.el6
condor-kbdd-8.4.0-1.2.osg33.el6
condor-procd-8.4.0-1.2.osg33.el6
condor-python-8.4.0-1.2.osg33.el6
condor-std-universe-8.4.0-1.2.osg33.el6
condor-test-8.4.0-1.2.osg33.el6
condor-vm-gahp-8.4.0-1.2.osg33.el6
edg-mkgridmap-4.0.3-2.osg33.el6
gip-1.3.11-7.osg33.el6
glideinwms-3.2.11.2-4.osg33.el6
glideinwms-common-tools-3.2.11.2-4.osg33.el6
glideinwms-condor-common-config-3.2.11.2-4.osg33.el6
glideinwms-factory-3.2.11.2-4.osg33.el6
glideinwms-factory-condor-3.2.11.2-4.osg33.el6
glideinwms-glidecondor-tools-3.2.11.2-4.osg33.el6
glideinwms-libs-3.2.11.2-4.osg33.el6
glideinwms-minimal-condor-3.2.11.2-4.osg33.el6
glideinwms-usercollector-3.2.11.2-4.osg33.el6
glideinwms-userschedd-3.2.11.2-4.osg33.el6
glideinwms-vofrontend-3.2.11.2-4.osg33.el6
glideinwms-vofrontend-standalone-3.2.11.2-4.osg33.el6
gridftp-hdfs-0.5.4-22.osg33.el6
gridftp-hdfs-debuginfo-0.5.4-22.osg33.el6
gums-1.5.1-1.osg33.el6
gums-client-1.5.1-1.osg33.el6
gums-service-1.5.1-1.osg33.el6
htcondor-ce-1.16-1.osg33.el6
htcondor-ce-client-1.16-1.osg33.el6
htcondor-ce-collector-1.16-1.osg33.el6
htcondor-ce-condor-1.16-1.osg33.el6
htcondor-ce-debuginfo-1.16-1.osg33.el6
htcondor-ce-lsf-1.16-1.osg33.el6
htcondor-ce-pbs-1.16-1.osg33.el6
htcondor-ce-sge-1.16-1.osg33.el6
igtf-ca-certs-1.68-1.osg33.el6
jglobus-2.1.0-5.osg33.el6
lcmaps-plugins-mount-under-scratch-0.0.4-1.osg33.el6
lcmaps-plugins-mount-under-scratch-debuginfo-0.0.4-1.osg33.el6
osg-ca-certs-1.49-1.osg33.el6
osg-configure-1.2.2-1.osg33.el6
osg-configure-ce-1.2.2-1.osg33.el6
osg-configure-cemon-1.2.2-1.osg33.el6
osg-configure-condor-1.2.2-1.osg33.el6
osg-configure-gateway-1.2.2-1.osg33.el6
osg-configure-gip-1.2.2-1.osg33.el6
osg-configure-gratia-1.2.2-1.osg33.el6
osg-configure-infoservices-1.2.2-1.osg33.el6
osg-configure-lsf-1.2.2-1.osg33.el6
osg-configure-managedfork-1.2.2-1.osg33.el6
osg-configure-misc-1.2.2-1.osg33.el6
osg-configure-monalisa-1.2.2-1.osg33.el6
osg-configure-network-1.2.2-1.osg33.el6
osg-configure-pbs-1.2.2-1.osg33.el6
osg-configure-rsv-1.2.2-1.osg33.el6
osg-configure-sge-1.2.2-1.osg33.el6
osg-configure-slurm-1.2.2-1.osg33.el6
osg-configure-squid-1.2.2-1.osg33.el6
osg-configure-tests-1.2.2-1.osg33.el6
osg-test-1.4.30-1.osg33.el6
osg-tested-internal-3.3-5.osg33.el6
osg-version-3.3.2-1.osg33.el6
privilege-xacml-2.6.5-1.osg33.el6
rsv-3.10.4-1.osg33.el6
rsv-consumers-3.10.4-1.osg33.el6
rsv-core-3.10.4-1.osg33.el6
rsv-metrics-3.10.4-1.osg33.el6
stashcache-0.6-1.osg33.el6
stashcache-cache-server-0.6-1.osg33.el6
stashcache-daemon-0.6-1.osg33.el6
stashcache-origin-server-0.6-1.osg33.el6
xrootd-4.2.3-1.osg33.el6
xrootd-client-4.2.3-1.osg33.el6
xrootd-client-devel-4.2.3-1.osg33.el6
xrootd-client-libs-4.2.3-1.osg33.el6
xrootd-debuginfo-4.2.3-1.osg33.el6
xrootd-devel-4.2.3-1.osg33.el6
xrootd-doc-4.2.3-1.osg33.el6
xrootd-fuse-4.2.3-1.osg33.el6
xrootd-libs-4.2.3-1.osg33.el6
xrootd-private-devel-4.2.3-1.osg33.el6
xrootd-python-4.2.3-1.osg33.el6
xrootd-selinux-4.2.3-1.osg33.el6
xrootd-server-4.2.3-1.osg33.el6
xrootd-server-devel-4.2.3-1.osg33.el6
xrootd-server-libs-4.2.3-1.osg33.el6
```

#### Enterprise Linux 7

``` file
axis-1.4-23.1.osg33.el7
axis-javadoc-1.4-23.1.osg33.el7
axis-manual-1.4-23.1.osg33.el7
blahp-1.18.14.bosco-1.osg33.el7
blahp-debuginfo-1.18.14.bosco-1.osg33.el7
condor-8.4.0-1.2.osg33.el7
condor-all-8.4.0-1.2.osg33.el7
condor-bosco-8.4.0-1.2.osg33.el7
condor-classads-8.4.0-1.2.osg33.el7
condor-classads-devel-8.4.0-1.2.osg33.el7
condor-debuginfo-8.4.0-1.2.osg33.el7
condor-kbdd-8.4.0-1.2.osg33.el7
condor-procd-8.4.0-1.2.osg33.el7
condor-python-8.4.0-1.2.osg33.el7
condor-test-8.4.0-1.2.osg33.el7
condor-vm-gahp-8.4.0-1.2.osg33.el7
edg-mkgridmap-4.0.3-2.osg33.el7
gip-1.3.11-7.osg33.el7
glideinwms-3.2.11.2-4.osg33.el7
glideinwms-common-tools-3.2.11.2-4.osg33.el7
glideinwms-condor-common-config-3.2.11.2-4.osg33.el7
glideinwms-factory-3.2.11.2-4.osg33.el7
glideinwms-factory-condor-3.2.11.2-4.osg33.el7
glideinwms-glidecondor-tools-3.2.11.2-4.osg33.el7
glideinwms-libs-3.2.11.2-4.osg33.el7
glideinwms-minimal-condor-3.2.11.2-4.osg33.el7
glideinwms-usercollector-3.2.11.2-4.osg33.el7
glideinwms-userschedd-3.2.11.2-4.osg33.el7
glideinwms-vofrontend-3.2.11.2-4.osg33.el7
glideinwms-vofrontend-standalone-3.2.11.2-4.osg33.el7
htcondor-ce-1.16-1.osg33.el7
htcondor-ce-client-1.16-1.osg33.el7
htcondor-ce-collector-1.16-1.osg33.el7
htcondor-ce-condor-1.16-1.osg33.el7
htcondor-ce-debuginfo-1.16-1.osg33.el7
htcondor-ce-lsf-1.16-1.osg33.el7
htcondor-ce-pbs-1.16-1.osg33.el7
htcondor-ce-sge-1.16-1.osg33.el7
igtf-ca-certs-1.68-1.osg33.el7
javamail-1.5.0-6.osg33.el7
javamail-javadoc-1.5.0-6.osg33.el7
lcmaps-plugins-mount-under-scratch-0.0.4-1.osg33.el7
lcmaps-plugins-mount-under-scratch-debuginfo-0.0.4-1.osg33.el7
osg-ca-certs-1.49-1.osg33.el7
osg-configure-1.2.2-1.osg33.el7
osg-configure-ce-1.2.2-1.osg33.el7
osg-configure-cemon-1.2.2-1.osg33.el7
osg-configure-condor-1.2.2-1.osg33.el7
osg-configure-gateway-1.2.2-1.osg33.el7
osg-configure-gip-1.2.2-1.osg33.el7
osg-configure-gratia-1.2.2-1.osg33.el7
osg-configure-infoservices-1.2.2-1.osg33.el7
osg-configure-lsf-1.2.2-1.osg33.el7
osg-configure-managedfork-1.2.2-1.osg33.el7
osg-configure-misc-1.2.2-1.osg33.el7
osg-configure-monalisa-1.2.2-1.osg33.el7
osg-configure-network-1.2.2-1.osg33.el7
osg-configure-pbs-1.2.2-1.osg33.el7
osg-configure-rsv-1.2.2-1.osg33.el7
osg-configure-sge-1.2.2-1.osg33.el7
osg-configure-slurm-1.2.2-1.osg33.el7
osg-configure-squid-1.2.2-1.osg33.el7
osg-configure-tests-1.2.2-1.osg33.el7
osg-test-1.4.30-1.osg33.el7
osg-tested-internal-3.3-5.osg33.el7
osg-version-3.3.2-1.osg33.el7
rsv-3.10.4-1_clipped.osg33.el7
rsv-consumers-3.10.4-1_clipped.osg33.el7
rsv-core-3.10.4-1_clipped.osg33.el7
rsv-metrics-3.10.4-1_clipped.osg33.el7
stashcache-0.6-1.osg33.el7
stashcache-cache-server-0.6-1.osg33.el7
stashcache-daemon-0.6-1.osg33.el7
stashcache-origin-server-0.6-1.osg33.el7
wsdl4j-1.6.3-3.1.osg33.el7
wsdl4j-javadoc-1.6.3-3.1.osg33.el7
xrootd-4.2.3-1.osg33.el7
xrootd-client-4.2.3-1.osg33.el7
xrootd-client-devel-4.2.3-1.osg33.el7
xrootd-client-libs-4.2.3-1.osg33.el7
xrootd-debuginfo-4.2.3-1.osg33.el7
xrootd-devel-4.2.3-1.osg33.el7
xrootd-doc-4.2.3-1.osg33.el7
xrootd-fuse-4.2.3-1.osg33.el7
xrootd-libs-4.2.3-1.osg33.el7
xrootd-private-devel-4.2.3-1.osg33.el7
xrootd-python-4.2.3-1.osg33.el7
xrootd-selinux-4.2.3-1.osg33.el7
xrootd-server-4.2.3-1.osg33.el7
xrootd-server-devel-4.2.3-1.osg33.el7
xrootd-server-libs-4.2.3-1.osg33.el7
```

### Upcoming Packages

We added or updated the following packages to the **upcoming** OSG yum repository. Note that in some cases, there are multiple RPMs for each package. You can click on any given package to see the set of RPMs or see the complete list below.

#### Enterprise Linux 6

#### Enterprise Linux 7

### Upcoming RPMs

If you wish to manually update your system, you can run yum update against the following packages:

If you wish to only update the RPMs that changed, the set of RPMs is:

#### Enterprise Linux 6

``` file
```

#### Enterprise Linux 7

``` file
```

OSG Software Release 3.3.3
==========================

**Release Date**: 2015-11-03

Summary of changes
------------------

This release contains:

-   CA certificates based on [IGTF 1.69](https://dist.eugridpma.info/distribution/igtf/current/CHANGES)

These [JIRA tickets](https://jira.opensciencegrid.org/issues/?jql=project%20%3D%20SOFTWARE%20AND%20fixVersion%20%3D%203.3.3%20ORDER%20BY%20priority%20DESC) were addressed in this release.

Detailed changes are below. All of the documentation can be found in the [Release3](https://twiki.grid.iu.edu/bin/view/Documentation/Release3/) area of the TWiki.

CA Certificate Update Information
---------------------------------

### What Is Changing

A new IGTF Certificate Bundle (v1.69) has just been released. This release has a very important change and we would like our sites to install it as soon as they can.

### Who Is Impacted By This Change:

All OSG sites and users need to install the new CA bundle. Especially US-Atlas and US-CMS sites should install the bundle as soon as possible since their VOs will go under the transition in November/December timeframe.

### Why This Change Is Happening:

The OSG CA is changing its backend service support from Digicert to CILogon HSM. As a result, a new OSG CA is created and just recently been accredited by IGTF. The official name of the new OSG CA in the IGTF bundle is CILogon OSG CA. Starting in November we will transition our VOs to start using the new OSG CA (CMS and Atlas being first ones). If a site has not installed the CA bundle by then, they will have authentication failures.

### What You Should Do:

Install the new CA bundle as soon as possible. The latest CA bundle will NOT be distributed in OSG Software v 3.1 because OSG no longer supports it.

For Linux servers (including worker nodes), ensure that the certificate bundle RPM is at version osg-ca-certs-1.50-1 or igtf-ca-certs-1.69-1 or greater.

Instructions for installing server CA certificate bundles are at <https://twiki.grid.iu.edu/bin/view/Documentation/Release3/InstallCertAuth>

We also highly recommend that you use the CA Cert automatic updater <https://twiki.grid.iu.edu/bin/view/Documentation/Release3/OsgCaCertsUpdater> but note that you need to be using a current OSG software distribution for that to work, that is, OSG 3.2 or 3.3.

### Other Information:

If you have the CA certificate bundle installed on a server with OSG 3.1, you need to upgrade to OSG 3.2 or greater. Follow these instructions: <https://twiki.grid.iu.edu/bin/view/Documentation/Release3/OSGReleaseSeries#Updating_from_OSG_3_1_or_3_2_to>

Please email OSG Security Team with questions or comments

Known Issues
------------

-   HTCondor 8.4.0 has changed it's behavior in ways that cause the GlideinWMS frontend configuration to break. In order to correct this, the following setting needs to be added to the configuration file:

        :::file
        COLLECTOR_USES_SHARED_PORT = False

-   StashCache packages need to be manually configured
    -   Manual configuration for origin server
        -   Assuming that the origin server connects only to a redirector (not directly to cache server), minimal xrootd configuration is required. The configuration file, /etc/xrootd/xrootd-stashcache-origin-server.cfg, in this release is overkill. Here are recommended settings to use:

                :::file
                xrd.port 1094
                all.role server
                all.manager stash-redirector.example.com 1213
                all.export / nostage
                xrootd.trace emsg login stall redirect
                ofs.trace none
                xrd.trace conn
                cms.trace all
                sec.protocol  host
                sec.protbind  * none
                all.adminpath /var/run/xrootd
                all.pidpath /var/run/xrootd

-   Manual configuration for cache server
    -   In contrast to the origin server configuration, one needs to declare `pss.origin <stash-redirector.example.com>` instead of configuring the cmsd or manager (only the xrootd daemon is required on the cache server). More detailed configuration of cache server for StashCache is [here](https://confluence.grid.iu.edu/pages/viewpage.action?title=Installing+an+XRootD+server+for+Stash+Cache&spaceKey=STAS).
-   In both cases, administrator needs to set the path of custom configuration file for its xrootd/cmds instance in /etc/sysconfig/xrootd, For example, change the cmds default from:

        :::file
        CMSD_DEFAULT_OPTIONS="-l /var/log/xrootd/cmsd.log -c /etc/xrootd/xrootd-clustered.cfg -k fifo"
<br/>

    to

        :::file
        CMSD_DEFAULT_OPTIONS="-l /var/log/xrootd/cmsd.log -c /etc/xrootd/xrootd-stashcache-origin-server.marian -k fifo" 

Updating to the new release
---------------------------

### Update Repositories

To update to this series, you need [install the current OSG repositories](../../common/yum#install-osg-repositories).

### Update Software

Once the new repositories are installed, you can update to this new release with:

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

#### Enterprise Linux 6

-   [igtf-ca-certs-1.69-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=igtf-ca-certs-1.69-1.osg33.el6)
-   [osg-ca-certs-1.50-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-ca-certs-1.50-1.osg33.el6)
-   [osg-version-3.3.3-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-version-3.3.3-1.osg33.el6)

#### Enterprise Linux 7

-   [igtf-ca-certs-1.69-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=igtf-ca-certs-1.69-1.osg33.el7)
-   [osg-ca-certs-1.50-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-ca-certs-1.50-1.osg33.el7)
-   [osg-version-3.3.3-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-version-3.3.3-1.osg33.el7)

### RPMs

If you wish to manually update your system, you can run yum update against the following packages:

    igtf-ca-certs osg-ca-certs osg-version

If you wish to only update the RPMs that changed, the set of RPMs is:

#### Enterprise Linux 6

``` file
igtf-ca-certs-1.69-1.osg33.el6
osg-ca-certs-1.50-1.osg33.el6
osg-version-3.3.3-1.osg33.el6
```

#### Enterprise Linux 7

``` file
igtf-ca-certs-1.69-1.osg33.el7
osg-ca-certs-1.50-1.osg33.el7
osg-version-3.3.3-1.osg33.el7
```

### Upcoming Packages

We added or updated the following packages to the **upcoming** OSG yum repository. Note that in some cases, there are multiple RPMs for each package. You can click on any given package to see the set of RPMs or see the complete list below.

#### Enterprise Linux 6

#### Enterprise Linux 7

### Upcoming RPMs

If you wish to manually update your system, you can run yum update against the following packages:

If you wish to only update the RPMs that changed, the set of RPMs is:

#### Enterprise Linux 6

``` file
```

#### Enterprise Linux 7

``` file
```

OSG Software Release 3.3.4
==========================

**Release Date**: 2015-11-10

Summary of changes
------------------

This release builds on the special OSG CA certificate release of November 3rd by making it easier to update from separate installations of the main CA certificate bundle and the new CILogon OSG CA. For hosts with only the main CA certificate bundle, there is no change.

This release contains:

-   Updated CVMFS configuration to support approved repositories from any domain
-   Fixed osg-ca-certs-updater to work when no "compat" packages are present
-   Added a new LCMAPS plug-in for process tracking
-   Added a new RSV PerfSONAR probe
-   Added a new RSV probe to check StashCache cache collector ads at the GOC
-   EL 7: Fixed Frontier Squid to start up properly after a reboot
-   EL 7: Fixed the MyProxy service to start up properly

These [JIRA tickets](https://jira.opensciencegrid.org/issues/?jql=project%20%3D%20SOFTWARE%20AND%20fixVersion%20%3D%203.3.4%20ORDER%20BY%20priority%20DESC) were addressed in this release.

Detailed changes are below. All of the documentation can be found in the [Release3](https://twiki.grid.iu.edu/bin/view/Documentation/Release3/) area of the TWiki.

Known Issues
------------

-   CILogon CA certificate files have been reorganized between our various CA certificate packages.  
To avoid file conflicts, you should upgrade your CA certificate RPMs at the same time, such as via the following command:  
<pre class="rootscreen">[root@client ~] $ yum update '\*-ca-cert\*'</pre>
-   When using osg-configure to configure a CE host, it will fail because it tries to contact a ReSS server that has been shut down permanently. The ReSS service has been deprecated since early 2014, and support for it will be removed from osg-configure in an upcoming version. To work around this osg-configure failure now, edit /etc/osg/config.d/30-infoservices.ini and set the option:

        :::file
        ress_servers = https://localhost[RAW]

-   There is a known memory leak in lcmaps-plugins-scas-client that HTCondor-CE triggers repeatedly. A special OSG release, coming soon, will fix the underlying memory leak, but in the meantime it is possible to make two small configuration changes to slow the rate at which memory leaks.
    1.  In /etc/condor-ce/config.d/01-ce-auth.conf, make sure the following attribute is defined as follows (note the $(USERS) at the end):

            :::file
            COLLECTOR.ALLOW_ADVERTISE_STARTD =  $(UNMAPPED_USERS), $(USERS)

1.  Also in /etc/condor-ce/config.d/01-ce-auth.conf, add the following line to the end of the file:

        ::: file
        GSS_ASSIST_GRIDMAP_CACHE_EXPIRATION = 30*$(MINUTE)

1.  Verify that the attributes are set properly by running the following command:

        ::: file
        $ condor_ce_config_val COLLECTOR.ALLOW_ADVERTISE_STARTD GSS_ASSIST_GRIDMAP_CACHE_EXPIRATION
        *@unmapped.opensciencegrid.org, *@users.opensciencegrid.org
        30*60

-   HTCondor 8.4.0 has changed it's behavior in ways that cause the GlideinWMS frontend configuration to break. In order to correct this, the following setting needs to be added to the configuration file:

        :::file
        COLLECTOR_USES_SHARED_PORT = False

-   StashCache packages need to be manually configured
    -   Manual configuration for origin server
        -   Assuming that the origin server connects only to a redirector (not directly to cache server), minimal xrootd configuration is required. The configuration file, /etc/xrootd/xrootd-stashcache-origin-server.cfg, in this release is overkill. Here are recommended settings to use:

                :::file
                xrd.port 1094
                all.role server
                all.manager stash-redirector.example.com 1213
                all.export / nostage
                xrootd.trace emsg login stall redirect
                ofs.trace none
                xrd.trace conn
                cms.trace all
                sec.protocol  host
                sec.protbind  * none
                all.adminpath /var/run/xrootd
                all.pidpath /var/run/xrootd

-   Manual configuration for cache server
    -   In contrast to the origin server configuration, one needs to declare `pss.origin <stash-redirector.example.com>` instead of configuring the cmsd or manager (only the xrootd daemon is required on the cache server). More detailed configuration of cache server for StashCache is [here](https://confluence.grid.iu.edu/pages/viewpage.action?title=Installing+an+XRootD+server+for+Stash+Cache&spaceKey=STAS).
-   In both cases, administrator needs to set the path of custom configuration file for its xrootd/cmds instance in /etc/sysconfig/xrootd, For example, change the cmds default from:

        :::file
        CMSD_DEFAULT_OPTIONS="-l /var/log/xrootd/cmsd.log -c /etc/xrootd/xrootd-clustered.cfg -k fifo"
<br/>

    to

        :::file
        CMSD_DEFAULT_OPTIONS="-l /var/log/xrootd/cmsd.log -c /etc/xrootd/xrootd-stashcache-origin-server.marian -k fifo" 

Updating to the new release
---------------------------

### Update Repositories

To update to this series, you need [install the current OSG repositories](../../common/yum#install-osg-repositories).

### Update Software

Once the new repositories are installed, you can update to this new release with:

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

#### Enterprise Linux 6

-   [cilogon-openid-ca-cert-1.1-3.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=cilogon-openid-ca-cert-1.1-3.osg33.el6)
-   [cilogon-osg-ca-cert-1.0-2.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=cilogon-osg-ca-cert-1.0-2.osg33.el6)
-   [cvmfs-config-osg-1.1-8.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=cvmfs-config-osg-1.1-8.osg33.el6)
-   [frontier-squid-2.7.STABLE9-24.2.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=frontier-squid-2.7.STABLE9-24.2.osg33.el6)
-   [jglobus-2.1.0-6.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=jglobus-2.1.0-6.osg33.el6)
-   [lcmaps-plugins-process-tracking-0.3-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=lcmaps-plugins-process-tracking-0.3-1.osg33.el6)
-   [myproxy-6.1.12-1.1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=myproxy-6.1.12-1.1.osg33.el6)
-   [osg-ca-certs-updater-1.3-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-ca-certs-updater-1.3-1.osg33.el6)
-   [osg-oasis-5-3.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-oasis-5-3.osg33.el6)
-   [osg-test-1.4.31-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-test-1.4.31-1.osg33.el6)
-   [osg-version-3.3.4-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-version-3.3.4-1.osg33.el6)
-   [rsv-3.12.0-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=rsv-3.12.0-1.osg33.el6)
-   [rsv-perfsonar-1.1.1-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=rsv-perfsonar-1.1.1-1.osg33.el6)

#### Enterprise Linux 7

-   [cilogon-openid-ca-cert-1.1-3.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=cilogon-openid-ca-cert-1.1-3.osg33.el7)
-   [cilogon-osg-ca-cert-1.0-2.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=cilogon-osg-ca-cert-1.0-2.osg33.el7)
-   [cvmfs-config-osg-1.1-8.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=cvmfs-config-osg-1.1-8.osg33.el7)
-   [frontier-squid-2.7.STABLE9-24.2.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=frontier-squid-2.7.STABLE9-24.2.osg33.el7)
-   [jglobus-2.1.0-6.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=jglobus-2.1.0-6.osg33.el7)
-   [lcmaps-plugins-process-tracking-0.3-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=lcmaps-plugins-process-tracking-0.3-1.osg33.el7)
-   [myproxy-6.1.12-1.1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=myproxy-6.1.12-1.1.osg33.el7)
-   [osg-ca-certs-updater-1.3-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-ca-certs-updater-1.3-1.osg33.el7)
-   [osg-oasis-5-3.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-oasis-5-3.osg33.el7)
-   [osg-test-1.4.31-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-test-1.4.31-1.osg33.el7)
-   [osg-version-3.3.4-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-version-3.3.4-1.osg33.el7)
-   [rsv-3.12.0-1\_clipped.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=rsv-3.12.0-1_clipped.osg33.el7)
-   [rsv-perfsonar-1.1.1-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=rsv-perfsonar-1.1.1-1.osg33.el7)

### RPMs

If you wish to manually update your system, you can run yum update against the following packages:

    cilogon-openid-ca-cert cilogon-osg-ca-cert cvmfs-config-osg frontier-squid frontier-squid-debuginfo jglobus lcmaps-plugins-process-tracking lcmaps-plugins-process-tracking-debuginfo myproxy myproxy-admin myproxy-debuginfo myproxy-devel myproxy-doc myproxy-libs myproxy-server myproxy-voms osg-ca-certs-updater osg-oasis osg-test osg-version rsv rsv-consumers rsv-core rsv-metrics rsv-perfsonar

If you wish to only update the RPMs that changed, the set of RPMs is:

#### Enterprise Linux 6

``` file
cilogon-openid-ca-cert-1.1-3.osg33.el6
cilogon-osg-ca-cert-1.0-2.osg33.el6
cvmfs-config-osg-1.1-8.osg33.el6
frontier-squid-2.7.STABLE9-24.2.osg33.el6
frontier-squid-debuginfo-2.7.STABLE9-24.2.osg33.el6
jglobus-2.1.0-6.osg33.el6
lcmaps-plugins-process-tracking-0.3-1.osg33.el6
lcmaps-plugins-process-tracking-debuginfo-0.3-1.osg33.el6
myproxy-6.1.12-1.1.osg33.el6
myproxy-admin-6.1.12-1.1.osg33.el6
myproxy-debuginfo-6.1.12-1.1.osg33.el6
myproxy-devel-6.1.12-1.1.osg33.el6
myproxy-doc-6.1.12-1.1.osg33.el6
myproxy-libs-6.1.12-1.1.osg33.el6
myproxy-server-6.1.12-1.1.osg33.el6
myproxy-voms-6.1.12-1.1.osg33.el6
osg-ca-certs-updater-1.3-1.osg33.el6
osg-oasis-5-3.osg33.el6
osg-test-1.4.31-1.osg33.el6
osg-version-3.3.4-1.osg33.el6
rsv-3.12.0-1.osg33.el6
rsv-consumers-3.12.0-1.osg33.el6
rsv-core-3.12.0-1.osg33.el6
rsv-metrics-3.12.0-1.osg33.el6
rsv-perfsonar-1.1.1-1.osg33.el6
```

#### Enterprise Linux 7

``` file
cilogon-openid-ca-cert-1.1-3.osg33.el7
cilogon-osg-ca-cert-1.0-2.osg33.el7
cvmfs-config-osg-1.1-8.osg33.el7
frontier-squid-2.7.STABLE9-24.2.osg33.el7
frontier-squid-debuginfo-2.7.STABLE9-24.2.osg33.el7
jglobus-2.1.0-6.osg33.el7
lcmaps-plugins-process-tracking-0.3-1.osg33.el7
lcmaps-plugins-process-tracking-debuginfo-0.3-1.osg33.el7
myproxy-6.1.12-1.1.osg33.el7
myproxy-admin-6.1.12-1.1.osg33.el7
myproxy-debuginfo-6.1.12-1.1.osg33.el7
myproxy-devel-6.1.12-1.1.osg33.el7
myproxy-doc-6.1.12-1.1.osg33.el7
myproxy-libs-6.1.12-1.1.osg33.el7
myproxy-server-6.1.12-1.1.osg33.el7
myproxy-voms-6.1.12-1.1.osg33.el7
osg-ca-certs-updater-1.3-1.osg33.el7
osg-oasis-5-3.osg33.el7
osg-test-1.4.31-1.osg33.el7
osg-version-3.3.4-1.osg33.el7
rsv-3.12.0-1_clipped.osg33.el7
rsv-consumers-3.12.0-1_clipped.osg33.el7
rsv-core-3.12.0-1_clipped.osg33.el7
rsv-metrics-3.12.0-1_clipped.osg33.el7
rsv-perfsonar-1.1.1-1.osg33.el7
```

### Upcoming Packages

We added or updated the following packages to the **upcoming** OSG yum repository. Note that in some cases, there are multiple RPMs for each package. You can click on any given package to see the set of RPMs or see the complete list below.

#### Enterprise Linux 6

#### Enterprise Linux 7

### Upcoming RPMs

If you wish to manually update your system, you can run yum update against the following packages:

If you wish to only update the RPMs that changed, the set of RPMs is:

#### Enterprise Linux 6

``` file
```

#### Enterprise Linux 7

``` file
```

OSG Software Release 3.3.5
==========================

**Release Date**: 2015-11-19

Summary of changes
------------------

This release contains:

-   lcmaps-plugins-scas-client
    -   Fixed a leak that caused HTCondor CE to use excessive memory
-   osg-configure
    -   Fixed a crash when attempting to connect to the recently retired ReSS servers while configuring a CE
    -   Now reconfigures the HTCondor CE after generating job environment files
-   HTCondor CE 1.20
    -   Users can now add onto accounting group defaults set by the job router
    -   Use GSI mapping cache to reduce calls to GSI
-   [HTCondor 8.4.2](https://www-auth.cs.wisc.edu/lists/htcondor-users/2015-November/msg00083.shtml)
-   RSV
    -   Corrected log rotation configuration error introduced in 3.2.30/3.3.4

These [JIRA tickets](https://jira.opensciencegrid.org/issues/?jql=project%20%3D%20SOFTWARE%20AND%20fixVersion%20%3D%203.3.5%20ORDER%20BY%20priority%20DESC) were addressed in this release.

Detailed changes are below. All of the documentation can be found in the [Release3](https://twiki.grid.iu.edu/bin/view/Documentation/Release3/) area of the TWiki.

Known Issues
------------

-   CILogon CA certificate files have been reorganized between our various CA certificate packages.  
To avoid file conflicts, you should upgrade your CA certificate RPMs at the same time, such as via the following command:  
<pre class="rootscreen">[root@client ~] $ yum update '\*-ca-cert\*'</pre>

<!-- -->

-   HTCondor 8.4.0 has changed it's behavior in ways that cause the GlideinWMS frontend configuration to break. In order to correct this, the following setting needs to be added to the configuration file:

        :::file
        COLLECTOR_USES_SHARED_PORT = False

-   StashCache packages need to be manually configured
    -   Manual configuration for origin server
        -   Assuming that the origin server connects only to a redirector (not directly to cache server), minimal xrootd configuration is required. The configuration file, /etc/xrootd/xrootd-stashcache-origin-server.cfg, in this release is overkill. Here are recommended settings to use:

                :::file
                xrd.port 1094
                all.role server
                all.manager stash-redirector.example.com 1213
                all.export / nostage
                xrootd.trace emsg login stall redirect
                ofs.trace none
                xrd.trace conn
                cms.trace all
                sec.protocol  host
                sec.protbind  * none
                all.adminpath /var/run/xrootd
                all.pidpath /var/run/xrootd

-   Manual configuration for cache server
    -   In contrast to the origin server configuration, one needs to declare `pss.origin <stash-redirector.example.com>` instead of configuring the cmsd or manager (only the xrootd daemon is required on the cache server). More detailed configuration of cache server for StashCache is [here](https://confluence.grid.iu.edu/pages/viewpage.action?title=Installing+an+XRootD+server+for+Stash+Cache&spaceKey=STAS).
-   In both cases, administrator needs to set the path of custom configuration file for its xrootd/cmds instance in /etc/sysconfig/xrootd, For example, change the cmds default from:

        :::file
        CMSD_DEFAULT_OPTIONS="-l /var/log/xrootd/cmsd.log -c /etc/xrootd/xrootd-clustered.cfg -k fifo"
<br/>

    to

        :::file
        CMSD_DEFAULT_OPTIONS="-l /var/log/xrootd/cmsd.log -c /etc/xrootd/xrootd-stashcache-origin-server.marian -k fifo" 

Updating to the new release
---------------------------

### Update Repositories

To update to this series, you need [install the current OSG repositories](../../common/yum#install-osg-repositories).

### Update Software

Once the new repositories are installed, you can update to this new release with:

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

#### Enterprise Linux 6

-   [blahp-1.18.15.bosco-3.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=blahp-1.18.15.bosco-3.osg33.el6)
-   [condor-8.4.2-1.1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=condor-8.4.2-1.1.osg33.el6)
-   [htcondor-ce-1.20-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=htcondor-ce-1.20-1.osg33.el6)
-   [lcmaps-plugins-scas-client-0.5.5-1.1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=lcmaps-plugins-scas-client-0.5.5-1.1.osg33.el6)
-   [osg-configure-1.2.4-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-configure-1.2.4-1.osg33.el6)
-   [osg-version-3.3.5-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-version-3.3.5-1.osg33.el6)
-   [rsv-3.12.5-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=rsv-3.12.5-1.osg33.el6)

#### Enterprise Linux 7

-   [blahp-1.18.15.bosco-3.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=blahp-1.18.15.bosco-3.osg33.el7)
-   [condor-8.4.2-1.1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=condor-8.4.2-1.1.osg33.el7)
-   [htcondor-ce-1.20-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=htcondor-ce-1.20-1.osg33.el7)
-   [lcmaps-plugins-scas-client-0.5.5-1.1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=lcmaps-plugins-scas-client-0.5.5-1.1.osg33.el7)
-   [osg-configure-1.2.4-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-configure-1.2.4-1.osg33.el7)
-   [osg-version-3.3.5-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-version-3.3.5-1.osg33.el7)
-   [rsv-3.12.5-1\_clipped.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=rsv-3.12.5-1_clipped.osg33.el7)

### RPMs

If you wish to manually update your system, you can run yum update against the following packages:

    blahp blahp-debuginfo condor condor-all condor-bosco condor-classads condor-classads-devel condor-cream-gahp condor-debuginfo condor-kbdd condor-procd condor-python condor-std-universe condor-test condor-vm-gahp htcondor-ce htcondor-ce-client htcondor-ce-collector htcondor-ce-condor htcondor-ce-debuginfo htcondor-ce-lsf htcondor-ce-pbs htcondor-ce-sge lcmaps-plugins-scas-client lcmaps-plugins-scas-client-debuginfo osg-configure osg-configure-ce osg-configure-cemon osg-configure-condor osg-configure-gateway osg-configure-gip osg-configure-gratia osg-configure-infoservices osg-configure-lsf osg-configure-managedfork osg-configure-misc osg-configure-monalisa osg-configure-network osg-configure-pbs osg-configure-rsv osg-configure-sge osg-configure-slurm osg-configure-squid osg-configure-tests osg-version rsv rsv-consumers rsv-core rsv-metrics

If you wish to only update the RPMs that changed, the set of RPMs is:

#### Enterprise Linux 6

``` file
blahp-1.18.15.bosco-3.osg33.el6
blahp-debuginfo-1.18.15.bosco-3.osg33.el6
condor-8.4.2-1.1.osg33.el6
condor-all-8.4.2-1.1.osg33.el6
condor-bosco-8.4.2-1.1.osg33.el6
condor-classads-8.4.2-1.1.osg33.el6
condor-classads-devel-8.4.2-1.1.osg33.el6
condor-cream-gahp-8.4.2-1.1.osg33.el6
condor-debuginfo-8.4.2-1.1.osg33.el6
condor-kbdd-8.4.2-1.1.osg33.el6
condor-procd-8.4.2-1.1.osg33.el6
condor-python-8.4.2-1.1.osg33.el6
condor-std-universe-8.4.2-1.1.osg33.el6
condor-test-8.4.2-1.1.osg33.el6
condor-vm-gahp-8.4.2-1.1.osg33.el6
htcondor-ce-1.20-1.osg33.el6
htcondor-ce-client-1.20-1.osg33.el6
htcondor-ce-collector-1.20-1.osg33.el6
htcondor-ce-condor-1.20-1.osg33.el6
htcondor-ce-debuginfo-1.20-1.osg33.el6
htcondor-ce-lsf-1.20-1.osg33.el6
htcondor-ce-pbs-1.20-1.osg33.el6
htcondor-ce-sge-1.20-1.osg33.el6
lcmaps-plugins-scas-client-0.5.5-1.1.osg33.el6
lcmaps-plugins-scas-client-debuginfo-0.5.5-1.1.osg33.el6
osg-configure-1.2.4-1.osg33.el6
osg-configure-ce-1.2.4-1.osg33.el6
osg-configure-cemon-1.2.4-1.osg33.el6
osg-configure-condor-1.2.4-1.osg33.el6
osg-configure-gateway-1.2.4-1.osg33.el6
osg-configure-gip-1.2.4-1.osg33.el6
osg-configure-gratia-1.2.4-1.osg33.el6
osg-configure-infoservices-1.2.4-1.osg33.el6
osg-configure-lsf-1.2.4-1.osg33.el6
osg-configure-managedfork-1.2.4-1.osg33.el6
osg-configure-misc-1.2.4-1.osg33.el6
osg-configure-monalisa-1.2.4-1.osg33.el6
osg-configure-network-1.2.4-1.osg33.el6
osg-configure-pbs-1.2.4-1.osg33.el6
osg-configure-rsv-1.2.4-1.osg33.el6
osg-configure-sge-1.2.4-1.osg33.el6
osg-configure-slurm-1.2.4-1.osg33.el6
osg-configure-squid-1.2.4-1.osg33.el6
osg-configure-tests-1.2.4-1.osg33.el6
osg-version-3.3.5-1.osg33.el6
rsv-3.12.5-1.osg33.el6
rsv-consumers-3.12.5-1.osg33.el6
rsv-core-3.12.5-1.osg33.el6
rsv-metrics-3.12.5-1.osg33.el6
```

#### Enterprise Linux 7

``` file
blahp-1.18.15.bosco-3.osg33.el7
blahp-debuginfo-1.18.15.bosco-3.osg33.el7
condor-8.4.2-1.1.osg33.el7
condor-all-8.4.2-1.1.osg33.el7
condor-bosco-8.4.2-1.1.osg33.el7
condor-classads-8.4.2-1.1.osg33.el7
condor-classads-devel-8.4.2-1.1.osg33.el7
condor-debuginfo-8.4.2-1.1.osg33.el7
condor-kbdd-8.4.2-1.1.osg33.el7
condor-procd-8.4.2-1.1.osg33.el7
condor-python-8.4.2-1.1.osg33.el7
condor-test-8.4.2-1.1.osg33.el7
condor-vm-gahp-8.4.2-1.1.osg33.el7
htcondor-ce-1.20-1.osg33.el7
htcondor-ce-client-1.20-1.osg33.el7
htcondor-ce-collector-1.20-1.osg33.el7
htcondor-ce-condor-1.20-1.osg33.el7
htcondor-ce-debuginfo-1.20-1.osg33.el7
htcondor-ce-lsf-1.20-1.osg33.el7
htcondor-ce-pbs-1.20-1.osg33.el7
htcondor-ce-sge-1.20-1.osg33.el7
lcmaps-plugins-scas-client-0.5.5-1.1.osg33.el7
lcmaps-plugins-scas-client-debuginfo-0.5.5-1.1.osg33.el7
osg-configure-1.2.4-1.osg33.el7
osg-configure-ce-1.2.4-1.osg33.el7
osg-configure-cemon-1.2.4-1.osg33.el7
osg-configure-condor-1.2.4-1.osg33.el7
osg-configure-gateway-1.2.4-1.osg33.el7
osg-configure-gip-1.2.4-1.osg33.el7
osg-configure-gratia-1.2.4-1.osg33.el7
osg-configure-infoservices-1.2.4-1.osg33.el7
osg-configure-lsf-1.2.4-1.osg33.el7
osg-configure-managedfork-1.2.4-1.osg33.el7
osg-configure-misc-1.2.4-1.osg33.el7
osg-configure-monalisa-1.2.4-1.osg33.el7
osg-configure-network-1.2.4-1.osg33.el7
osg-configure-pbs-1.2.4-1.osg33.el7
osg-configure-rsv-1.2.4-1.osg33.el7
osg-configure-sge-1.2.4-1.osg33.el7
osg-configure-slurm-1.2.4-1.osg33.el7
osg-configure-squid-1.2.4-1.osg33.el7
osg-configure-tests-1.2.4-1.osg33.el7
osg-version-3.3.5-1.osg33.el7
rsv-3.12.5-1_clipped.osg33.el7
rsv-consumers-3.12.5-1_clipped.osg33.el7
rsv-core-3.12.5-1_clipped.osg33.el7
rsv-metrics-3.12.5-1_clipped.osg33.el7
```

### Upcoming Packages

We added or updated the following packages to the **upcoming** OSG yum repository. Note that in some cases, there are multiple RPMs for each package. You can click on any given package to see the set of RPMs or see the complete list below.

#### Enterprise Linux 6

#### Enterprise Linux 7

### Upcoming RPMs

If you wish to manually update your system, you can run yum update against the following packages:

If you wish to only update the RPMs that changed, the set of RPMs is:

#### Enterprise Linux 6

``` file
```

#### Enterprise Linux 7

``` file
```

OSG Software Release 3.3.6
==========================

**Release Date**: 2015-12-08

Summary of changes
------------------

This release contains:

-   Fixed a bug that prevented HTCondor 8.4's CREAM gahp from starting
-   CA certificates based on [IGTF 1.70](https://dist.eugridpma.info/distribution/igtf/current/CHANGES)
    -   Updated CRL URL hosted by KIT for ArmeSFO (AM)
    -   Added NorduGrid 2015 trust anchor (DK,NO,SE,FI,IS)
    -   Discontinued superseded DigiCertGridCA-1G2-Classic (US)
-   Restored the 'Sign AUP on behalf of user' feature in VOMS Admin Server

These [JIRA tickets](https://jira.opensciencegrid.org/issues/?jql=project%20%3D%20SOFTWARE%20AND%20fixVersion%20%3D%203.3.6%20ORDER%20BY%20priority%20DESC) were addressed in this release.

Detailed changes are below. All of the documentation can be found in the [Release3](https://twiki.grid.iu.edu/bin/view/Documentation/Release3/) area of the TWiki.

Known Issues
------------

-   The `osg-cert-request` and `osg-gridadmin-cert-request` may return `CILogon 500` errors when retrieving certificates. To fix this, follow the below instructions:
    1.  Save the following text (including the empty line at the end) to `~/osgpkitools.patch`:  
    <pre class="file">Index: OSGPKIUtils.py

**`===============================================================`** --- OSGPKIUtils.py (revision 22192) +++ OSGPKIUtils.py (revision 22193) @@ -362,6 +362,7 @@ \#

self.X509Request.set\_pubkey(pkey=self.PKey) + self.X509Request.set\_version(0) self.X509Request.sign(pkey=self.PKey, md='sha1') return self.X509Request

</pre>

1.  `cd` into the appropriate folder:  
<pre class="rootscreen">\# For EL5 hosts:

[root@client ~] $ cd /usr/lib/python2.4/site-packages/osgpkitools/ \# For EL6 hosts: [root@client ~] $ cd /usr/lib/python2.6/site-packages/osgpkitools/</pre>

1.  Apply the patch:  
<pre class="rootscreen">[root@client ~] $ patch < ~/osgpkitools.patch</pre> \* HTCondor 8.4.0 has changed it's behavior in ways that cause the GlideinWMS frontend configuration to break. In order to correct this, the following setting needs to be added to the configuration file:

        :::file
        COLLECTOR_USES_SHARED_PORT = False

-   StashCache packages need to be manually configured
    -   Manual configuration for origin server
        -   Assuming that the origin server connects only to a redirector (not directly to cache server), minimal xrootd configuration is required. The configuration file, /etc/xrootd/xrootd-stashcache-origin-server.cfg, in this release is overkill. Here are recommended settings to use:

                :::file
                xrd.port 1094
                all.role server
                all.manager stash-redirector.example.com 1213
                all.export / nostage
                xrootd.trace emsg login stall redirect
                ofs.trace none
                xrd.trace conn
                cms.trace all
                sec.protocol  host
                sec.protbind  * none
                all.adminpath /var/run/xrootd
                all.pidpath /var/run/xrootd

-   Manual configuration for cache server
    -   In contrast to the origin server configuration, one needs to declare `pss.origin <stash-redirector.example.com>` instead of configuring the cmsd or manager (only the xrootd daemon is required on the cache server). More detailed configuration of cache server for StashCache is [here](https://confluence.grid.iu.edu/pages/viewpage.action?title=Installing+an+XRootD+server+for+Stash+Cache&spaceKey=STAS).
-   In both cases, administrator needs to set the path of custom configuration file for its xrootd/cmds instance in /etc/sysconfig/xrootd, For example, change the cmds default from:

        :::file
        CMSD_DEFAULT_OPTIONS="-l /var/log/xrootd/cmsd.log -c /etc/xrootd/xrootd-clustered.cfg -k fifo"
<br/>

to

        :::file
        CMSD_DEFAULT_OPTIONS="-l /var/log/xrootd/cmsd.log -c /etc/xrootd/xrootd-stashcache-origin-server.marian -k fifo" 

Updating to the new release
---------------------------

### Update Repositories

To update to this series, you need [install the current OSG repositories](../../common/yum#install-osg-repositories).

### Update Software

Once the new repositories are installed, you can update to this new release with:

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

#### Enterprise Linux 6

-   [condor-8.4.2-1.2.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=condor-8.4.2-1.2.osg33.el6)
-   [gip-1.3.11-8.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=gip-1.3.11-8.osg33.el6)
-   [igtf-ca-certs-1.70-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=igtf-ca-certs-1.70-1.osg33.el6)
-   [osg-ca-certs-1.51-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-ca-certs-1.51-1.osg33.el6)
-   [osg-configure-1.2.5-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-configure-1.2.5-1.osg33.el6)
-   [osg-info-services-1.1.0-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-info-services-1.1.0-1.osg33.el6)
-   [osg-test-1.4.32-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-test-1.4.32-1.osg33.el6)
-   [osg-version-3.3.6-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-version-3.3.6-1.osg33.el6)
-   [voms-admin-server-2.7.0-1.17.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=voms-admin-server-2.7.0-1.17.osg33.el6)

#### Enterprise Linux 7

-   [condor-8.4.2-1.2.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=condor-8.4.2-1.2.osg33.el7)
-   [gip-1.3.11-8.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=gip-1.3.11-8.osg33.el7)
-   [igtf-ca-certs-1.70-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=igtf-ca-certs-1.70-1.osg33.el7)
-   [osg-ca-certs-1.51-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-ca-certs-1.51-1.osg33.el7)
-   [osg-configure-1.2.5-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-configure-1.2.5-1.osg33.el7)
-   [osg-info-services-1.1.0-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-info-services-1.1.0-1.osg33.el7)
-   [osg-test-1.4.32-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-test-1.4.32-1.osg33.el7)
-   [osg-version-3.3.6-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-version-3.3.6-1.osg33.el7)

### RPMs

If you wish to manually update your system, you can run yum update against the following packages:

    condor condor-all condor-bosco condor-classads condor-classads-devel condor-cream-gahp condor-debuginfo condor-kbdd condor-procd condor-python condor-std-universe condor-test condor-vm-gahp gip igtf-ca-certs osg-ca-certs osg-configure osg-configure-ce osg-configure-cemon osg-configure-condor osg-configure-gateway osg-configure-gip osg-configure-gratia osg-configure-infoservices osg-configure-lsf osg-configure-managedfork osg-configure-misc osg-configure-monalisa osg-configure-network osg-configure-pbs osg-configure-rsv osg-configure-sge osg-configure-slurm osg-configure-squid osg-configure-tests osg-info-services osg-test osg-version voms-admin-server

If you wish to only update the RPMs that changed, the set of RPMs is:

#### Enterprise Linux 6

``` file
condor-8.4.2-1.2.osg33.el6
condor-all-8.4.2-1.2.osg33.el6
condor-bosco-8.4.2-1.2.osg33.el6
condor-classads-8.4.2-1.2.osg33.el6
condor-classads-devel-8.4.2-1.2.osg33.el6
condor-cream-gahp-8.4.2-1.2.osg33.el6
condor-debuginfo-8.4.2-1.2.osg33.el6
condor-kbdd-8.4.2-1.2.osg33.el6
condor-procd-8.4.2-1.2.osg33.el6
condor-python-8.4.2-1.2.osg33.el6
condor-std-universe-8.4.2-1.2.osg33.el6
condor-test-8.4.2-1.2.osg33.el6
condor-vm-gahp-8.4.2-1.2.osg33.el6
gip-1.3.11-8.osg33.el6
igtf-ca-certs-1.70-1.osg33.el6
osg-ca-certs-1.51-1.osg33.el6
osg-configure-1.2.5-1.osg33.el6
osg-configure-ce-1.2.5-1.osg33.el6
osg-configure-cemon-1.2.5-1.osg33.el6
osg-configure-condor-1.2.5-1.osg33.el6
osg-configure-gateway-1.2.5-1.osg33.el6
osg-configure-gip-1.2.5-1.osg33.el6
osg-configure-gratia-1.2.5-1.osg33.el6
osg-configure-infoservices-1.2.5-1.osg33.el6
osg-configure-lsf-1.2.5-1.osg33.el6
osg-configure-managedfork-1.2.5-1.osg33.el6
osg-configure-misc-1.2.5-1.osg33.el6
osg-configure-monalisa-1.2.5-1.osg33.el6
osg-configure-network-1.2.5-1.osg33.el6
osg-configure-pbs-1.2.5-1.osg33.el6
osg-configure-rsv-1.2.5-1.osg33.el6
osg-configure-sge-1.2.5-1.osg33.el6
osg-configure-slurm-1.2.5-1.osg33.el6
osg-configure-squid-1.2.5-1.osg33.el6
osg-configure-tests-1.2.5-1.osg33.el6
osg-info-services-1.1.0-1.osg33.el6
osg-test-1.4.32-1.osg33.el6
osg-version-3.3.6-1.osg33.el6
voms-admin-server-2.7.0-1.17.osg33.el6
```

#### Enterprise Linux 7

``` file
condor-8.4.2-1.2.osg33.el7
condor-all-8.4.2-1.2.osg33.el7
condor-bosco-8.4.2-1.2.osg33.el7
condor-classads-8.4.2-1.2.osg33.el7
condor-classads-devel-8.4.2-1.2.osg33.el7
condor-debuginfo-8.4.2-1.2.osg33.el7
condor-kbdd-8.4.2-1.2.osg33.el7
condor-procd-8.4.2-1.2.osg33.el7
condor-python-8.4.2-1.2.osg33.el7
condor-test-8.4.2-1.2.osg33.el7
condor-vm-gahp-8.4.2-1.2.osg33.el7
gip-1.3.11-8.osg33.el7
igtf-ca-certs-1.70-1.osg33.el7
osg-ca-certs-1.51-1.osg33.el7
osg-configure-1.2.5-1.osg33.el7
osg-configure-ce-1.2.5-1.osg33.el7
osg-configure-cemon-1.2.5-1.osg33.el7
osg-configure-condor-1.2.5-1.osg33.el7
osg-configure-gateway-1.2.5-1.osg33.el7
osg-configure-gip-1.2.5-1.osg33.el7
osg-configure-gratia-1.2.5-1.osg33.el7
osg-configure-infoservices-1.2.5-1.osg33.el7
osg-configure-lsf-1.2.5-1.osg33.el7
osg-configure-managedfork-1.2.5-1.osg33.el7
osg-configure-misc-1.2.5-1.osg33.el7
osg-configure-monalisa-1.2.5-1.osg33.el7
osg-configure-network-1.2.5-1.osg33.el7
osg-configure-pbs-1.2.5-1.osg33.el7
osg-configure-rsv-1.2.5-1.osg33.el7
osg-configure-sge-1.2.5-1.osg33.el7
osg-configure-slurm-1.2.5-1.osg33.el7
osg-configure-squid-1.2.5-1.osg33.el7
osg-configure-tests-1.2.5-1.osg33.el7
osg-info-services-1.1.0-1.osg33.el7
osg-test-1.4.32-1.osg33.el7
osg-version-3.3.6-1.osg33.el7
```

### Upcoming Packages

We added or updated the following packages to the **upcoming** OSG yum repository. Note that in some cases, there are multiple RPMs for each package. You can click on any given package to see the set of RPMs or see the complete list below.

#### Enterprise Linux 6

#### Enterprise Linux 7

### Upcoming RPMs

If you wish to manually update your system, you can run yum update against the following packages:

If you wish to only update the RPMs that changed, the set of RPMs is:

#### Enterprise Linux 6

``` file
```

#### Enterprise Linux 7

``` file
```

OSG Software Release 3.3.7
==========================

**Release Date**: 2015-12-15

Summary of changes
------------------

This release contains:

-   Fixed a problem in the PKI command line tools where "Certificate Requests" were being rejected by the CILogon Certificate Authority.

These [JIRA tickets](https://jira.opensciencegrid.org/issues/?jql=project%20%3D%20SOFTWARE%20AND%20fixVersion%20%3D%203.3.7%20ORDER%20BY%20priority%20DESC) were addressed in this release.

Detailed changes are below. All of the documentation can be found in the [Release3](https://twiki.grid.iu.edu/bin/view/Documentation/Release3/) area of the TWiki.

Known Issues
------------

-   HTCondor 8.4.0 has changed it's behavior in ways that cause the GlideinWMS frontend configuration to break. In order to correct this, the following setting needs to be added to the configuration file:

        :::file
        COLLECTOR_USES_SHARED_PORT = False

-   StashCache packages need to be manually configured
    -   Manual configuration for origin server
        -   Assuming that the origin server connects only to a redirector (not directly to cache server), minimal xrootd configuration is required. The configuration file, /etc/xrootd/xrootd-stashcache-origin-server.cfg, in this release is overkill. Here are recommended settings to use:

                :::file
                xrd.port 1094
                all.role server
                all.manager stash-redirector.example.com 1213
                all.export / nostage
                xrootd.trace emsg login stall redirect
                ofs.trace none
                xrd.trace conn
                cms.trace all
                sec.protocol  host
                sec.protbind  * none
                all.adminpath /var/run/xrootd
                all.pidpath /var/run/xrootd

-   Manual configuration for cache server
    -   In contrast to the origin server configuration, one needs to declare `pss.origin <stash-redirector.example.com>` instead of configuring the cmsd or manager (only the xrootd daemon is required on the cache server). More detailed configuration of cache server for StashCache is [here](https://confluence.grid.iu.edu/pages/viewpage.action?title=Installing+an+XRootD+server+for+Stash+Cache&spaceKey=STAS).
-   In both cases, administrator needs to set the path of custom configuration file for its xrootd/cmds instance in /etc/sysconfig/xrootd, For example, change the cmds default from:

        :::file
        CMSD_DEFAULT_OPTIONS="-l /var/log/xrootd/cmsd.log -c /etc/xrootd/xrootd-clustered.cfg -k fifo"
<br/>

    to

        :::file
        CMSD_DEFAULT_OPTIONS="-l /var/log/xrootd/cmsd.log -c /etc/xrootd/xrootd-stashcache-origin-server.marian -k fifo" 

Updating to the new release
---------------------------

### Update Repositories

To update to this series, you need [install the current OSG repositories](../../common/yum#install-osg-repositories).

### Update Software

Once the new repositories are installed, you can update to this new release with:

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

#### Enterprise Linux 6

-   [osg-pki-tools-1.2.13-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-pki-tools-1.2.13-1.osg33.el6)
-   [osg-version-3.3.7-1.osg33.el6](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-version-3.3.7-1.osg33.el6)

#### Enterprise Linux 7

-   [osg-pki-tools-1.2.13-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-pki-tools-1.2.13-1.osg33.el7)
-   [osg-version-3.3.7-1.osg33.el7](https://koji-hub.batlab.org/koji/search?match=glob&type=build&terms=osg-version-3.3.7-1.osg33.el7)

### RPMs

If you wish to manually update your system, you can run yum update against the following packages:

    osg-pki-tools osg-pki-tools-tests osg-version

If you wish to only update the RPMs that changed, the set of RPMs is:

#### Enterprise Linux 6

``` file
osg-pki-tools-1.2.13-1.osg33.el6
osg-pki-tools-tests-1.2.13-1.osg33.el6
osg-version-3.3.7-1.osg33.el6
```

#### Enterprise Linux 7

``` file
osg-pki-tools-1.2.13-1.osg33.el7
osg-pki-tools-tests-1.2.13-1.osg33.el7
osg-version-3.3.7-1.osg33.el7
```

### Upcoming Packages

We added or updated the following packages to the **upcoming** OSG yum repository. Note that in some cases, there are multiple RPMs for each package. You can click on any given package to see the set of RPMs or see the complete list below.

#### Enterprise Linux 6

#### Enterprise Linux 7

### Upcoming RPMs

If you wish to manually update your system, you can run yum update against the following packages:

If you wish to only update the RPMs that changed, the set of RPMs is:

#### Enterprise Linux 6

``` file
```

#### Enterprise Linux 7

``` file
```

