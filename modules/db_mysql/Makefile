# $Id$
#
# WARNING: do not run this directly, it should be run by the master Makefile

include ../../Makefile.defs
auto_gen=
NAME=db_mysql.so

SCRIPTS_DIR?=../../scripts/

# set CROSS_COMPILE to true if you want to skip
# the autodetection
# CROSS_COMPILE=true

ifeq ($(CROSS_COMPILE),)
HAS_MYSQLCFG=$(shell if which mysql_config >/dev/null 2>/dev/null;then echo YES; fi)
endif

ifeq ($(HAS_MYSQLCFG),YES)

	# use autodetection
	DEFS += $(shell mysql_config --include | sed 's/\(-I.*\)\/mysql/\1/g' )
	LIBS = $(shell mysql_config --libs)

else

	# use standard know paths
	# mysql.h locations (freebsd,openbsd  solaris)
	DEFS +=-I$(LOCALBASE)/include -I$(LOCALBASE)/include/mysql \
		-I$(LOCALBASE)/mysql/include/mysql -I$(LOCALBASE)/mysql/include \
		-I$(SYSBASE)/include/mysql

	# libmysqlclient locations on RH/Suse, Solaris /OpenBSD, FreeBSD
	# (Debian does the right thing and puts it in /usr/lib)
	LIBS=-L$(SYSBASE)/lib/mysql -L$(LOCALBASE)/lib -L$(LOCALBASE)/lib/mysql \
		-L$(LOCALBASE)/mysql/lib/mysql/ \
		-L$(LOCALBASE)/mysql/lib \
		-L$(SYSBASE)/lib64/mysql \
		-lm -lmysqlclient -lz

endif

include ../../Makefile.modules

install_module_custom: 
	mkdir -p $(modules-prefix)/$(lib-dir)/opensipsctl ; \
	sed -e "s#/usr/local/sbin#$(bin-target)#g" \
		< $(SCRIPTS_DIR)/opensipsctl.mysql > /tmp/opensipsctl.mysql ; \
	$(INSTALL_CFG) /tmp/opensipsctl.mysql \
		$(modules-prefix)/$(lib-dir)/opensipsctl/opensipsctl.mysql ; \
	rm -fr /tmp/opensipsctl.mysql ; \
	sed -e "s#/usr/local/share/opensips#$(data-target)#g" \
	< $(SCRIPTS_DIR)/opensipsdbctl.mysql > /tmp/opensipsdbctl.mysql ; \
	$(INSTALL_TOUCH) $(modules-prefix)/$(lib-dir)/opensipsctl/opensipsdbctl.mysql ; \
	$(INSTALL_CFG) /tmp/opensipsdbctl.mysql $(modules-prefix)/$(lib-dir)/opensipsctl/ ; \
	rm -fr /tmp/opensipsdbctl.mysql ; \
	mkdir -p $(data-prefix)/$(data-dir)/mysql ; \
	for FILE in $(wildcard $(SCRIPTS_DIR)/mysql/*) ; do \
		if [ -f $$FILE ] ; then \
		$(INSTALL_TOUCH) $$FILE \
			$(data-prefix)/$(data-dir)/mysql/`basename "$$FILE"` ; \
		$(INSTALL_CFG) $$FILE \
			$(data-prefix)/$(data-dir)/mysql/`basename "$$FILE"` ; \
		fi ;\
	done ; \
