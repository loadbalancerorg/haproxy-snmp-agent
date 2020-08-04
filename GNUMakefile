all: libl7info.so libl7stat.so

%.c: %.re2c
	re2c $< -o $@
%.so: %.c
	c99 -fPIC -shared `net-snmp-config --cflags` $< -o $@
