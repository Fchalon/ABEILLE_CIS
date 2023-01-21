ansible-project-compliance-rhel7-cis
================

Configure RHEL/Centos 7 machine to be [CIS](https://www.cisecurity.org/cis-benchmarks/) compliant. Level 1 and 2 findings will be corrected by default, unless in Check mode.

Unless in Check mode, this role **will make changes to the system** that could break things. This is not an auditing tool but rather a remediation tool to be used after an audit has been conducted.

Based on [CIS RedHat Enterprise Linux 7 Benchmark v2.2.0.1 (https://www.cisecurity.org/cis-benchmarks/).

This repo originated from work done by [MindPoint Group](https://github.com/mindpointgroup) and [Sam Doran](https://github.com/samdoran/ansible-role-stig)

Using this repository
------------
- Standard Usage:
  - Add this repo to your Ansible Tower as the Project "RHEL7 CIS" (or other name as appropriate)
  - Create a Job Template under this project that uses the existing ansible-playbook-compliance-rhel7-cis playbook
  - Configure any desired variables in the extra variables section (just copy the text from defaults/main.yml as a starting point)
  - Launch
- Custom Usage:
  - Simply download (clone) the repository and start modifying files according to your needs.

Requirements
------------
You should carefully read through the tasks to make sure these changes will not break your systems before running this playbook.
If you want to do a dry run without changing anything, set the below sections (rhel7cis_section1-6) to false or enable ansible check mode.

Dependencies
------------
Ansible > 2.2

Log Messages
------------
IBM changes include the addition of manual verification checks and compliance status messages.  The compliance status messages will not appear if a role variable has caused the condition for that sub task to be false or may not appear if the preliminary results determine the sub task is not applicable.  The playbook results will record those as skipped.  

**Message types include**:
- SUCCESS_COMPLIANT: Benchmark met
- NOT_COMPLIANT: Change required, benchmark not met
- MANUAL_CHECK_REQUIRED (NOT_AUTOMATED|SKIPPED): No check was performed and a manual execution and review is required
- MANUAL_CHECK_REQUIRED (MANUAL_VERIFICATION_REQUIRED): Manual review of data collected is required

Variables
--------------
There are many variables defined in defaults/main.yml. This list shows the most important.

**rhel7cis_notauto**: Run CIS checks that we typically do NOT want to automate due to the high probability of breaking the system (Default: false)

**rhel7cis_section1**: CIS - General Settings (Section 1) (Default: true)

**rhel7cis_section2**: CIS - Services settings (Section 2) (Default: true)

**rhel7cis_section3**: CIS - Network settings (Section 3) (Default: true)

**rhel7cis_section4**: CIS - Logging and Auditing settings (Section 4) (Default: true)

**rhel7cis_section5**: CIS - Access, Authentication and Authorization settings (Section 5) (Default: true)

**rhel7cis_section6**: CIS - System Maintenance settings (Section 6) (Default: true)  

##### Disable all selinux functions
`rhel7cis_selinux_disable: false`

##### Service variables:
###### These control whether a server should or should not be allowed to continue to run these services

```
rhel7cis_avahi_server: false  
rhel7cis_cups_server: false  
rhel7cis_dhcp_server: false  
rhel7cis_ldap_server: false  
rhel7cis_telnet_server: false  
rhel7cis_nfs_server: false  
rhel7cis_rpc_server: false  
rhel7cis_ntalk_server: false  
rhel7cis_rsyncd_server: false  
rhel7cis_tftp_server: false  
rhel7cis_rsh_server: false  
rhel7cis_nis_server: false  
rhel7cis_snmp_server: false  
rhel7cis_squid_server: false  
rhel7cis_smb_server: false  
rhel7cis_dovecot_server: false  
rhel7cis_httpd_server: false  
rhel7cis_vsftpd_server: false  
rhel7cis_named_server: false  
rhel7cis_bind: false  
rhel7cis_vsftpd: false  
rhel7cis_httpd: false  
rhel7cis_dovecot: false  
rhel7cis_samba: false  
rhel7cis_squid: false  
rhel7cis_net_snmp: false  
```  

##### Designate server as a Mail server
`rhel7cis_is_mail_server: false`


##### System network parameters (host only OR host and router)
`rhel7cis_is_router: false`  


##### IPv6 required
`rhel7cis_ipv6_required: true`  


##### AIDE
`rhel7cis_config_aide: true`

###### AIDE cron settings
```
rhel7cis_aide_cron:
  cron_user: root
  cron_file: /etc/crontab
  aide_job: '/usr/sbin/aide --check'
  aide_minute: 0
  aide_hour: 5
  aide_day: '*'
  aide_month: '*'
  aide_weekday: '*'  
```

##### SELinux policy
`rhel7cis_selinux_pol: targeted`


##### Set to 'true' if X Windows is needed in your environment
`rhel7cis_xwindows_required: no`


##### Client application requirements
```
rhel7cis_openldap_clients_required: false
rhel7cis_telnet_required: false
rhel7cis_talk_required: false  
rhel7cis_rsh_required: false
rhel7cis_ypbind_required: false
```

##### Time Synchronization
```
rhel7cis_time_synchronization: chrony
rhel7cis_time_Synchronization: ntp

rhel7cis_time_synchronization_servers:
    - 0.pool.ntp.org
    - 1.pool.ntp.org
    - 2.pool.ntp.org
    - 3.pool.ntp.org  
```  

##### 3.4.2 | PATCH | Ensure /etc/hosts.allow is configured
```
rhel7cis_host_allow:
  - "10.0.0.0/255.0.0.0"  
  - "172.16.0.0/255.240.0.0"  
  - "192.168.0.0/255.255.0.0"    
```  

```
rhel7cis_firewall: firewalld
rhel7cis_firewall: iptables
```

Tags
----
Many tags are available for precise control of what is and is not changed.

Some examples of using tags:

```
    # Audit and patch the site
    ansible-playbook site.yml --tags="patch"
```

License
-------

MIT


# License
* [Central CE license for internal company assets](https://github.kyndryl.net/Continuous-Engineering/cloud-based-automation-of-licenses-and-software-assets/blob/master/LICENSE.md)
