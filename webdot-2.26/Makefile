DESTDIR=

# Location for the webdot cgi program
CGI-BIN_DIR=/var/www/cgi-bin

# Location for some example web pages using webdot
HTML_DIR=/var/www/html

# A place that the webdot cgi program can cache its generated images
# (make install creates a webdot subdirectory in this dir.)
CACHE_DIR=/var/cache

# The uid:gid in effect when cgi-bin programs are running, only this
# user should be able to read/write the webdot cache.
HTTPD-USER-GROUP=apache:apache

# Location of tclsh8.3 (or later) executable
TCLSH_EXECUTABLE=/usr/bin/tclsh8.3

# Direct reference to libtcldot.so to avoid directory searching overhead
# of tcl package mechanism.
LIBTCLDOT=/usr/lib/graphviz/libtcldot.so.0.0.0

# Ghostscript is used for pdf output format
GS=/usr/bin/gs

# ps2epsi for epsi output format
PS2EPSI=/usr/bin/ps2epsi

# LOCALHOSTONLY set to 0 to allow conversion of graphs from other hosts
#      warning: use only if you want to provide a public graph server
#      this can result in uncontrolled load on your system
LOCALHOSTONLY=1

# version number for webdot-*.tar.gz
VERSION=2.26
TOP_DIR=/home/gviz/tmp/gviz/webdot
BUILD_DIR=/home/gviz/tmp/gviz/webdot

###############################################################

.PHONY: all genfiles install uninstall distdir dist clean

all: genfiles

genfiles:
	tclsh linktut.tcl
	tclsh genindex.tcl

install:
	mkdir -p $(DESTDIR)$(CGI-BIN_DIR) $(DESTDIR)$(HTML_DIR)
	echo "#!$(TCLSH_EXECUTABLE)" > $(DESTDIR)$(CGI-BIN_DIR)/webdot
	echo "set LIBTCLDOT $(LIBTCLDOT)" >> $(DESTDIR)$(CGI-BIN_DIR)/webdot
	echo "set CACHE_ROOT $(CACHE_DIR)/webdot" >> $(DESTDIR)$(CGI-BIN_DIR)/webdot
	echo "set GS $(GS)" >> $(DESTDIR)$(CGI-BIN_DIR)/webdot
	echo "set PS2EPSI $(PS2EPSI)" >> $(DESTDIR)$(CGI-BIN_DIR)/webdot
	echo "set LOCALHOSTONLY  $(LOCALHOSTONLY)" >> $(DESTDIR)$(CGI-BIN_DIR)/webdot
	cat ${BUILD_DIR}/cgi-bin/webdot >> $(DESTDIR)$(CGI-BIN_DIR)/webdot
	cp ${BUILD_DIR}/cgi-bin/webdot.tclet $(DESTDIR)$(CGI-BIN_DIR)/webdot.tcl
	chmod +x $(DESTDIR)$(CGI-BIN_DIR)/webdot
	cp -r ${BUILD_DIR}/html/webdot $(DESTDIR)$(HTML_DIR)/
	rm -rf $(DESTDIR)$(CACHE_DIR)/webdot
	mkdir -p $(DESTDIR)$(CACHE_DIR)/webdot
	chmod 700 $(DESTDIR)$(CACHE_DIR)/webdot
	chown $(HTTPD-USER-GROUP) $(DESTDIR)$(CACHE_DIR)/webdot

uninstall:
	rm -f $(DESTDIR)$(CGI-BIN_DIR)/webdot
	rm -f $(DESTDIR)$(CGI-BIN_DIR)/webdot.tclet
	rm -rf $(DESTDIR)$(HTML_DIR)/webdot
	rm -rf $(DESTDIR)$(CACHE_DIR)/webdot

distdir=webdot-$(VERSION)

dist: genfiles
	rm -rf $(distdir)*
	mkdir -p $(distdir)/cgi-bin $(distdir)/config $(distdir)/html.in
	mkdir -p $(distdir)/html/webdot/graphs/directed $(distdir)/html/webdot/graphs/undirected
	cp AUTHORS CHANGES COPYING INSTALL README $(distdir)/
	cp configure.ac configure Makefile.in Makefile webdot.spec.in webdot.spec $(distdir)/
	cp genindex.tcl linktut.tcl scaffold.tcl $(distdir)/
	cp config/config.guess config/config.sub config/install-sh $(distdir)/config/
	cp cgi-bin/webdot $(distdir)/cgi-bin/
	cp cgi-bin/webdot.tclet $(distdir)/cgi-bin/
	cp html/webdot/*.html $(distdir)/html/webdot/
	cp html/webdot/*.dot $(distdir)/html/webdot/
	cp html/webdot/*.png $(distdir)/html/webdot/
	cp html/webdot/*.gif $(distdir)/html/webdot/
	cp html.in/*.html.in $(distdir)/html.in/
	cp html.in/*.dot.in $(distdir)/html.in/
	cp html/webdot/graphs/directed/*dot $(distdir)/html/webdot/graphs/directed/
	cp html/webdot/graphs/undirected/*dot $(distdir)/html/webdot/graphs/undirected/
	tar cf - $(distdir) | gzip > $(distdir).tar.gz
	rm -rf $(distdir)

clean:
	( cd html.in; for i in *.in; do rm -f ../html/webdot/$${i%%.in}; done )
	rm -f html/webdot/demo.html
