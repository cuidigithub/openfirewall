#
# $Id: Makefile 7884 2015-02-09 16:54:04Z owes $
#

COMPILE=$(CC) $(CFLAGS) -D_GNU_SOURCE -I../installer/
OBJLIBS=setuid.o ../installer/helper.o

PROGS = iowrap logwatch
SUID_PROGS = accountingctrl setfwrules emailhelper  \
	restartdhcp restartsquid restartssh ofwreboot \
	ofwbkcfg installpackage installfcdsl ipsecctrl \
	red setaliases ofwbackup restartshaping restartntpd \
	setdate rebuildhosts rebuildlangtexts conntrack_helper \
	restartsyslogd sysinfo restartopenvpn iptableswrapper
BACKUP_PROGS = ofwrestore

all : helper.o helper_backup.o $(PROGS) $(SUID_PROGS) $(BACKUP_PROGS)

install :
	install -m 755  $(PROGS) /usr/local/bin
	install -m 4750 -g nobody $(SUID_PROGS) /usr/local/bin
	install -m 4750 -g nobody $(BACKUP_PROGS) /usr/local/bin

######

% : %.c setuid.o ../installer/helper.o
	$(COMPILE) $< $(OBJLIBS) -o $@

helper.o :
	$(MAKE) -C ../installer helper
helper_backup.o :
	$(COMPILE) ../installer/helper_backup.c -c -o $@
setuid.o: setuid.c setuid.h
	$(COMPILE) $< -c -o $@

$(SUID_PROGS): setuid.o ../installer/helper.o

$(BACKUP_PROGS): setuid.o ../installer/helper.o ../installer/helper_backup.o

$(PROGS): ../installer/helper.o

ofwbkcfg: ofwbkcfg.c setuid.o
	$(COMPILE) -lcrypt ofwbkcfg.c setuid.o -o $@

ofwrestore: setuid.o ../installer/helper.o ../installer/helper_backup.o
	$(COMPILE) ofwrestore.c $(OBJLIBS) ../installer/helper_backup.o -o $@

