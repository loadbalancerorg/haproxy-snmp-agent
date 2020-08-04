## Build requirements:

The following binaries are required to build:

Program          | Source
-----------------|-----------------------
net-snmp-config  | package net-snmp-devel
g++              | package gcc-g++
c99              | package gcc
re2c             | https://re2c.org/




## To build on CentOS 6:

1. Run: `yum install net-snmp-devel gcc-c++ --disableexcludes=all`
2. Download and build re2c.  A simple `./configure && make && make install` should work
3. Change to the directory containing the re2c files and run: make all

You should have shared libraries libl7info.so and libl7stat.so




## To install the agents on CentOS 6:

1. Copy libl7info.so and libl7stat.so to /usr/lib64 (or wherever)
2. Add the following two lines to /etc/snmp/snmpd.conf:

    dlmod l7info /usr/lib64/libl7info.so
    dlmod l7stat /usr/lib64/libl7stat.so

3. Run: service snmpd restart




## To install the MIBs for use by the NetSNMP client utilities:

1. Copy the *-MIB.txt files to /usr/share/snmp/mibs
2. Add the following line to /etc/snmp/snmp.conf:

    mibs +LBO-MIB:L7INFO-EXPERIMENTAL-MIB:L7STAT-EXPERIMENTAL-MIB




## To debug the agents (on server; not required during normal operation):

1. Run: service snmpd stop 
2. Run: snmpd -f -Le -Dl7stat,l7info | tee snmpd.log
3. Press Ctrl-C when done collecting logs
4. Run: service snmpd start 




## To test the agents:

    snmpwalk  -v 2c -c "${community_string}" "${host}" -OQ l7Info | less -S
    snmptable -v 2c -c "${community_string}" "${host}" -Cib l7ProxyServer | less -S
    snmptable -v 2c -c "${community_string}" "${host}" -Cib l7Frontend | less -S
    snmptable -v 2c -c "${community_string}" "${host}" -Cib l7Backend | less -S
    snmptable -v 2c -c "${community_string}" "${host}" -Cib l7Server | less -S
