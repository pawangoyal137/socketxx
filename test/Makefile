srcdir = .

CC  = gcc
CXX = g++

CFLAGS   = -g -O2
CXXFLAGS = -Wall -g -O2 -g -O0
LDFLAGS  = 

LIBS = -L$(srcdir)/.. -L$(srcdir)/../socket++/.libs -lsocket++ 

TESTSRCS = tdunread.C tdunwrite.C \
	tdinread.C tdinwrite.C \
	tsunread.C tsunwrite.C \
	tsinread.C tsinwrite.C \
	tpipe.C \
	tsockpair.C \
	tpopen.C tpopen-lpc.C tpopen-ftp.C \
	tpopentest.C \
	testall.C \
	tnntp.C \
	tsmtp.C \
	tsendfiles.C \
	thostnames.C \
	twhois.C \
	tfork.C tfork0.C \
	tsecho.C tcecho.C \
	tcsmtp.C \
        tcftp.C \
	tbind-0.C tbind-1.C \
        tsig-0.C \
	test_sockerr.C \
	simple_httpd.C

TESTOBJS = $(TESTSRCS:.C=.o)
TESTS = $(TESTSRCS:.C=)

DEPEND_SOURCES = $(srcdir)/*.C

.SUFFIXES: .o .C .cc

.cc.o .C.o:
	$(CXX) -c $(CXXFLAGS) -I$(srcdir) -I$(srcdir)/.. -I$(srcdir)/../socket++ $<

.PHONY: check
check: testall.out
	diff -c testall.exp testall.out || make -f Makefile.in message

message:
	@echo
	@echo If there are any differences, manually check whether
	@echo testall.out and testall.exp have the same lines but
	@echo only their order is different. If they have the same
	@echo lines but for the order, you have installed properly.
	@echo Otherwise, try to findout what might be the reason and
	@echo if you could not please email me with your problem at
	@echo gs4t@virginia.edu
	@echo


testall.out: $(TESTS)
	./testall > testall.out

$(TESTS): $(TESTOBJS)
	$(CXX) $(CXXFLAGS) -o $@ $@.o $(LIBS)

update-version:
	for i in $(TESTSRCS); \
	do \
	  sed -e "s/Version: [0-9a-zA-Z]* [0-9.]*/Version: `date +%d%h%y` $(VERSION)/g" $$i > TMP$$i; \
	  mv TMP$$i $$i; \
	done

clean:
	-rm -f *~ *.o core $(TESTS)

distclean: clean
	-rm -f Makefile

distcopy:
	mkdir -p ../socket++-$(VERSION)/test
	for f in $(TESTSRCS) Makefile.in testall.exp; \
	do \
	 (cd ../socket++-$(VERSION)/test; ln -s ../../test/$$f $$f) \
	done

depend:
	makedepend $(CXXINCLUDES) $(TESTSRCS)
