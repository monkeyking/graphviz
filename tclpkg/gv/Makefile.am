# $Id$ $Revision$
## Process this file with automake to produce Makefile.in

SUBDIRS = demo

pdfdir = $(pkgdatadir)/doc/pdf

AM_CPPFLAGS = \
	-I$(top_srcdir) \
	-I$(top_srcdir)/lib/gvc \
	-I$(top_srcdir)/lib/common \
	-I$(top_srcdir)/lib/cgraph \
	-I$(top_srcdir)/lib/cdt \
	-I$(top_srcdir)/lib/pathplan

LIBS = -lc

BASESOURCES = gv.cpp gv.i gv_builtins.c
BASELIBS = $(top_builddir)/lib/gvc/libgvc.la \
	$(top_builddir)/lib/cgraph/libcgraph.la \
	$(top_builddir)/lib/cdt/libcdt.la
BASECPPFLAGS = $(AM_CPPFLAGS) -DDEMAND_LOADING=1

pkgsharpdir = $(pkglibdir)/sharp
SHARP_data = gv.cs gvPINVOKE.cs SWIGTYPE_p_Agraph_t.cs SWIGTYPE_p_Agnode_t.cs SWIGTYPE_p_Agedge_t.cs SWIGTYPE_p_Agsym_t.cs SWIGTYPE_p_FILE.cs
nodist_libgv_sharp_la_SOURCES = gv_sharp.cpp $(SHARP_data)
libgv_sharp_la_SOURCES = $(BASESOURCES) gv_dummy_init.c
libgv_sharp_la_LIBADD = $(BASELIBS) $(SHARP_LIBS) 
libgv_sharp_la_LDFLAGS = -module -avoid-version
libgv_sharp_la_CPPFLAGS = $(BASECPPFLAGS) $(SHARP_INCLUDES)
$(SHARP_data): gv_sharp.cpp
gv_sharp.cpp: gv.i
	$(SWIG) -c++ -csharp -namespace gv -o gv_sharp.cpp $(srcdir)/gv.i

pkggodir = $(pkglibdir)/go
GO_data = runtime.h gv.go
nodist_libgv_go_la_SOURCES = gv_go.cpp runtime.h gv.go
libgv_go_la_SOURCES = $(BASESOURCES) gv_dummy_init.c
libgv_go_la_LIBADD = $(BASELIBS) $(GO_LIBS)
libgv_go_la_LDFLAGS = -module -avoid-version
libgv_go_la_CPPFLAGS = $(BASECPPFLAGS) $(GO_INCLUDES)
gv_gc.c gv.go: gv_go.cpp
gv_go.cpp: gv.i
	$(SWIG) -c++ -go -intgosize $(INTGOSIZE) -o gv_go.cpp $(srcdir)/gv.i 
	mv $@ $@.orig
	sed '/include/s/malloc\.h/cstdlib/' $@.orig > $@
	rm $@.orig
runtime.h:
	$(SWIG) -c++ -go -intgosize $(INTGOSIZE) -external-runtime runtime.h
gv.go:
	echo "package gv" >gv.go
	

gvgotest: gv.i gvgotest.go gv.go runtime.h
	( \
	eval $(go env); \
	mkdir -p go/src/gv; \
	$(SED) -e 's/#include "gvc.h"/#include <graphviz\/gvc.h>/' <$(srcdir)/gv.i >go/src/gv/gv.swigcxx; \
	$(SED) -e 's/#include <gvc.h>/#include <graphviz\/gvc.h>/' <$(srcdir)/gv_dummy_init.c >go/src/gv/gv_dummy_init.c; \
	$(SED) -e 's/#include "gvplugin.h"/#include <graphviz\/gvplugin.h>/' <$(srcdir)/gv_builtins.c >go/src/gv/gv_builtins.c; \
	$(SED) -e 's/#include "gvc.h"/#include <graphviz\/gvc.h>/' -e 's/DEMAND_LOADING/1/' <$(srcdir)/gv.cpp >go/src/gv/gv.cpp; \
	cat gv.go >go/src/gv/gv.go; \
	cat $(srcdir)/gvgotest.go >go/src/gvgotest.go; \
	cd go/src; \
	GOPATH=/home/ellson/FIX/Linux.x86_64/build/graphviz/tclpkg/gv/go \
	go build -x -work -gccgoflags -lgvc -compiler gccgo gvgotest.go && mv gvgotest ../../; \
	)

#        cat $(srcdir)/gv_gc.c >go/src/gv/gv_gc.c; \
#        cat runtime.h >go/src/gv/runtime.h; \
#

pkgguiledir = $(pkglibdir)/guile
GUILE_data = 
nodist_libgv_guile_la_SOURCES = gv_guile.cpp $(GUILE_data)
libgv_guile_la_SOURCES = $(BASESOURCES) gv_dummy_init.c
libgv_guile_la_LIBADD = $(BASELIBS) $(GUILE_LIBS)
libgv_guile_la_LDFLAGS = -module -avoid-version
libgv_guile_la_CPPFLAGS = $(BASECPPFLAGS) $(GUILE_INCLUDES)
# $(GUILE_data): gv_guile.cpp
gv_guile.cpp: gv.i
	$(SWIG) -c++ -guile -o gv_guile.cpp $(srcdir)/gv.i
	$(SED) -e 's/SWIG_GUILE_INIT_STATIC void/void/' <gv_guile.cpp >t$$
	mv -f t$$ gv_guile.cpp

pkgiodir = $(pkglibdir)/io
IO_data = 
nodist_libgv_io_la_SOURCES = gv_io.cpp $(IO_data)
libgv_io_la_SOURCES = $(BASESOURCES) gv_dummy_init.c
libgv_io_la_LIBADD = $(BASELIBS) $(IO_LIBS)
libgv_io_la_LDFLAGS = -module -avoid-version
libgv_io_la_CPPFLAGS = $(BASECPPFLAGS) $(IO_INCLUDES)
# $(IO_data): gv_io.cpp
gv_io.cpp: gv.i
	$(SWIG) -c++ -io -o gv_io.cpp $(srcdir)/gv.i

libjavadir = $(pkglibdir)/java
pkgjavadir = $(libjavadir)/org/graphviz
JAVA_data = gv.java gvJNI.java SWIGTYPE_p_Agraph_t.java SWIGTYPE_p_Agnode_t.java SWIGTYPE_p_Agedge_t.java SWIGTYPE_p_Agsym_t.java SWIGTYPE_p_FILE.java
nodist_libgv_java_la_SOURCES = gv_java.cpp
libgv_java_la_SOURCES = $(BASESOURCES)  gv_java_init.c
libgv_java_la_LIBADD = $(BASELIBS) $(JAVA_LIBS)
libgv_java_la_LDFLAGS = -module -avoid-version $(JNI_EXTRA_LDFLAGS)
libgv_java_la_CPPFLAGS = $(BASECPPFLAGS) $(SWIGJAVA_CPPFLAGS) $(JAVA_INCLUDES)
$(JAVA_data): gv_java.cpp
gv_java.cpp: gv.i
	$(SWIG) -package org.graphviz -java -c++ -o gv_java.cpp $(srcdir)/gv.i

pkgluadir = $(pkglibdir)/lua
LUA_data = 
nodist_libgv_lua_la_SOURCES = gv_lua.cpp $(LUA_data)
libgv_lua_la_SOURCES = $(BASESOURCES) gv_dummy_init.c
libgv_lua_la_LIBADD = $(BASELIBS) $(LUA_LIBS)
libgv_lua_la_LDFLAGS = -module -avoid-version
libgv_lua_la_CPPFLAGS = $(BASECPPFLAGS) $(LUA_INCLUDES)
# $(LUA_data): gv_lua.cpp
gv_lua.cpp: gv.i
	$(SWIG) -c++ -lua -o gv_lua.cpp $(srcdir)/gv.i
	mv $@ $@.orig
	sed '/include/s/malloc\.h/cstdlib/' $@.orig > $@
	rm $@.orig

pkgocamldir = $(pkglibdir)/ocaml
OCAML_data = gv.cmo gv.cma gv.cmi META.gv
#OCAML_data += gv.mli
if WITH_OCAMLOPT
#OCAML_data += gv.a gv.cmx gv.cmxa
endif
nodist_libgv_ocaml_la_SOURCES = gv_ocaml.cpp $(OCAML_data)
libgv_ocaml_la_SOURCES = $(BASESOURCES) gv_dummy_init.c
libgv_ocaml_la_LIBADD = $(BASELIBS) $(OCAML_LIBS)
libgv_ocaml_la_LDFLAGS = -module -avoid-version
libgv_ocaml_la_CPPFLAGS = $(BASECPPFLAGS) $(OCAML_INCLUDES) -Dint64=int64_t
gv.ml gv.mli: gv_ocaml.cpp
gv_ocaml.cpp: gv.i
	$(SWIG) -c++ -ocaml -o gv_ocaml.cpp $(srcdir)/gv.i
	mv $@ $@.orig
	sed '/int caml_array_length/d' $@.orig > $@
	rm $@.orig
swig.mli:
	$(SWIG) -ocaml -co swig.mli
swig.ml:
	$(SWIG) -ocaml -co swig.ml
swig.cmi: swig.mli
	ocamlc -c swig.mli
swig.cmo: swig.ml swig.cmi
	ocamlc -c swig.ml
gv.cmi: gv.mli swig.cmi
	ocamlc -c gv.mli
gv.cmo: gv.ml gv.cmi
	ocamlc -c gv.ml
gv.cma: swig.cmo gv.cmo
	ocamlc -a -dllib dllgv.so -custom -o gv.cma swig.cmo gv.cmo
gv.cmx: gv.ml gv.cma
	ocamlopt -c gv.ml
gv.cmxa: gv.cmx
	ocamlopt -a -o gv.cmxa gv.cmx
gv.a:	gv.cmxa

pkgperldir = $(pkglibdir)/perl
PERL        ?= perl
# cleanup for opencsw's perl which adds flags that g++ doesn't like
# use sed (because I don't know perl) to cleanup flags that clang doen't like
PERL_CC      = `$(PERL) -MExtUtils::Embed -e ccopts | $(SED) 's/-fstack-clash-protection //g' | $(PERL) -pe 's/-x.*? //g'`
PERL_LD      = `$(PERL) -MExtUtils::Embed -e ldopts`
VENDOR_ARCH  = `$(PERL) -MConfig -e 'print $$Config{installvendorarch};'`
PERL_data = gv.pm
nodist_libgv_perl_la_SOURCES = gv_perl.cpp $(PERL_data)
libgv_perl_la_SOURCES = $(BASESOURCES) gv_dummy_init.c
libgv_perl_la_LIBADD = $(BASELIBS) $(PERL_LIBS)
libgv_perl_la_LDFLAGS = -module -avoid-version $(PERL_LD)
libgv_perl_la_CPPFLAGS = $(BASECPPFLAGS) $(PERL_CC)
$(PERL_data): gv_perl.cpp
gv_perl.cpp: gv.i
	$(SWIG) -c++ $(SWIG_PERL_OPT) -o gv_perl.cpp $(srcdir)/gv.i

pkgphpdir = $(pkglibdir)/php
PHP_data = gv.php
nodist_libgv_php_la_SOURCES = gv_php.cpp php_gv.h
libgv_php_la_SOURCES = $(BASESOURCES) gv_php_init.c
libgv_php_la_LIBADD = $(BASELIBS) $(PHP_LIBS)
libgv_php_la_LDFLAGS = -module -avoid-version
libgv_php_la_CPPFLAGS = $(BASECPPFLAGS) $(PHP_INCLUDES)
gv.php: gv_php.cpp
php_gv.h: gv_php.cpp
gv_php.cpp: gv.i
	$(SWIG) -c++ $(SWIG_PHP_OPT) -o gv_php.cpp $(srcdir)/gv.i

pkgpythondir = $(pkglibdir)/python
PYTHON_data = gv.py
nodist_libgv_python_la_SOURCES = gv_python.cpp $(PYTHON_data)
libgv_python_la_SOURCES = $(BASESOURCES) gv_dummy_init.c
libgv_python_la_LIBADD = $(BASELIBS) $(PYTHON_LIBS)
libgv_python_la_LDFLAGS = -module -avoid-version
libgv_python_la_CPPFLAGS = $(BASECPPFLAGS) $(PYTHON_INCLUDES)
$(PYTHON_data): gv_python.cpp
gv_python.cpp: gv.i
	$(SWIG) -c++ -python -o gv_python.cpp $(srcdir)/gv.i

pkgpython2dir = $(pkglibdir)/python2
PYTHON2_data = gv.py
nodist_libgv_python2_la_SOURCES = gv_python2.cpp $(PYTHON2_data)
libgv_python2_la_SOURCES = $(BASESOURCES) gv_dummy_init.c
libgv_python2_la_LIBADD = $(BASELIBS) $(PYTHON2_LIBS)
libgv_python2_la_LDFLAGS = -module -avoid-version
libgv_python2_la_CPPFLAGS = $(BASECPPFLAGS) $(PYTHON2_INCLUDES)
$(PYTHON2_data): gv_python2.cpp
gv_python2.cpp: gv.i
	$(SWIG) -c++ -python -o gv_python2.cpp $(srcdir)/gv.i

pkgpython3dir = $(pkglibdir)/python3
PYTHON3_data = gv.py
nodist_libgv_python3_la_SOURCES = gv_python3.cpp $(PYTHON3_data)
libgv_python3_la_SOURCES = $(BASESOURCES) gv_dummy_init.c
libgv_python3_la_LIBADD = $(BASELIBS) $(PYTHON3_LIBS)
libgv_python3_la_LDFLAGS = -module -avoid-version
libgv_python3_la_CPPFLAGS = $(BASECPPFLAGS) $(PYTHON3_INCLUDES)
$(PYTHON3_data): gv_python3.cpp
gv_python3.cpp: gv.i
	$(SWIG) -c++ -python -o gv_python3.cpp $(srcdir)/gv.i

pkgRdir = $(pkglibdir)/R
R_data = 
nodist_libgv_R_la_SOURCES = gv_R.cpp $(R_data)
libgv_R_la_SOURCES = $(BASESOURCES) gv_dummy_init.c
libgv_R_la_LIBADD = $(BASELIBS) $(R_LIBS)
libgv_R_la_LDFLAGS = -module -avoid-version
libgv_R_la_CPPFLAGS = $(BASECPPFLAGS) $(R_CFLAGS)
# $(R_data): gv_R.cpp
gv_R.cpp: gv.i
	$(SWIG) -c++ -r -o gv_R.cpp $(srcdir)/gv.i

pkgrubydir = $(pkglibdir)/ruby
RUBY_data = 
nodist_libgv_ruby_la_SOURCES = gv_ruby.cpp $(RUBY_data)
libgv_ruby_la_SOURCES = $(BASESOURCES) gv_dummy_init.c
libgv_ruby_la_LIBADD = $(BASELIBS) $(RUBY_LIBS)
libgv_ruby_la_LDFLAGS = -module -avoid-version
libgv_ruby_la_CPPFLAGS = $(BASECPPFLAGS) $(RUBY_CFLAGS)
# $(RUBY_data): gv_ruby.cpp
gv_ruby.cpp: gv.i
	$(SWIG) -c++ -ruby -o gv_ruby.cpp $(srcdir)/gv.i

pkgtcldir = $(pkglibdir)/tcl
TCL_data = 
nodist_libgv_tcl_la_SOURCES = gv_tcl.cpp $(TCL_data)
libgv_tcl_la_SOURCES = $(BASESOURCES) gv_tcl_init.c
libgv_tcl_la_LIBADD = $(top_builddir)/tclpkg/tclstubs/libtclstubs_C.la $(BASELIBS) $(TCL_LIBS)
libgv_tcl_la_LDFLAGS = -module -avoid-version
libgv_tcl_la_CPPFLAGS = $(BASECPPFLAGS) $(TCL_INCLUDES)
# $(TCL_data): gv_tcl.cpp
gv_tcl.cpp: gv.i
	$(SWIG) -c++ -tcl -namespace -o gv_tcl.cpp $(srcdir)/gv.i

pkginclude_HEADERS = gv.i gv.cpp
pkglib_LTLIBRARIES = 
man_MANS =
pdf_DATA = 
DEVTSTS =
if WITH_SWIG
if WITH_SHARP
pkgsharp_DATA = $(SHARP_data)
man_MANS += gv.3sharp
pdf_DATA += gv.3sharp.pdf
pkgsharp_LTLIBRARIES = libgv_sharp.la
DEVTSTS += test_sharp
endif
if WITH_GO
pkggo_DATA = $(GO_data)
man_MANS += gv.3go
pdf_DATA += gv.3go.pdf
pkggo_LTLIBRARIES = libgv_go.la
DEVTSTS += test_go
endif
if WITH_GUILE
pkgguile_DATA = $(GUILE_data)
man_MANS += gv.3guile
pdf_DATA += gv.3guile.pdf
pkgguile_LTLIBRARIES = libgv_guile.la
DEVTSTS += test_guile
endif
if WITH_IO
pkgio_DATA = $(IO_data)
man_MANS += gv.3io
pdf_DATA += gv.3io.pdf
pkgio_LTLIBRARIES = libgv_io.la
DEVTSTS += test_io
endif
if WITH_JAVA
pkgjava_DATA = $(JAVA_data)
man_MANS += gv.3java
pdf_DATA += gv.3java.pdf
pkgjava_LTLIBRARIES = libgv_java.la
DEVTSTS += test_java
endif
if WITH_LUA
pkglua_DATA = $(LUA_data)
man_MANS += gv.3lua
pdf_DATA += gv.3lua.pdf
pkglua_LTLIBRARIES = libgv_lua.la
DEVTSTS += test_lua
endif
if WITH_OCAML
pkgocaml_DATA = $(OCAML_data)
man_MANS += gv.3ocaml
pdf_DATA += gv.3ocaml.pdf
pkgocaml_LTLIBRARIES = libgv_ocaml.la
DEVTSTS += test_ocaml
endif
if WITH_PERL
pkgperl_DATA = $(PERL_data)
man_MANS += gv.3perl
pdf_DATA += gv.3perl.pdf
pkgperl_LTLIBRARIES = libgv_perl.la
DEVTSTS += test_perl
endif
if WITH_PHP
pkgphp_DATA = $(PHP_data)
man_MANS += gv.3php
pdf_DATA += gv.3php.pdf
pkgphp_LTLIBRARIES = libgv_php.la
DEVTSTS += test_php
endif
if WITH_PYTHON
pkgpython_DATA = $(PYTHON_data)
pkgpython_LTLIBRARIES = libgv_python.la
DEVTSTS += test_python
endif
if WITH_PYTHON2
pkgpython2_DATA = $(PYTHON2_data)
pkgpython2_LTLIBRARIES = libgv_python2.la
DEVTSTS += test_python2
endif
if WITH_PYTHON3
pkgpython3_DATA = $(PYTHON3_data)
pkgpython3_LTLIBRARIES = libgv_python3.la
DEVTSTS += test_python3
endif
man_MANS += gv.3python
pdf_DATA += gv.3python.pdf
if WITH_R
pkgR_DATA = $(R_data)
man_MANS += gv.3r
pdf_DATA += gv.3r.pdf
pkgR_LTLIBRARIES = libgv_R.la
DEVTSTS += test_R
endif
if WITH_RUBY
pkgruby_DATA = $(RUBY_data)
man_MANS += gv.3ruby
pdf_DATA += gv.3ruby.pdf
pkgruby_LTLIBRARIES = libgv_ruby.la
DEVTSTS += test_ruby
endif
if WITH_TCL
pkgtcl_DATA = $(TCL_data)
man_MANS += gv.3tcl
pdf_DATA += gv.3tcl.pdf
pkgtcl_LTLIBRARIES = libgv_tcl.la
DEVTSTS += test_tcl

all-local: pkgIndex.tcl

pkgIndex.tcl: libgv_tcl.la
	sh $(top_srcdir)/tclpkg/mkpkgindex.sh libgv_tcl.la gv 0
endif
endif

man = gv.3sharp gv.3go gv.3guile gv.3io gv.3java gv.3lua gv.3ocaml \
	gv.3perl gv.3php gv.3python gv.3r gv.3ruby gv.3tcl

pdf = gv.3sharp.pdf gv.3go.pdf gv.3guile.pdf gv.3io.pdf gv.3java.pdf gv.3lua.pdf \
      gv.3ocaml.pdf gv.3perl.pdf gv.3php.pdf gv.3python.pdf gv.3r.pdf gv.3ruby.pdf gv.3tcl.pdf

$(man): gv.i gv_doc_writer.tcl gv_doc_template.tcl gv_doc_langs.tcl
	$(TCLSH) $(srcdir)/gv_doc_writer.tcl "$(srcdir)"

gv.3sharp.pdf: gv.3sharp
	- $(GROFF) -Tps -man gv.3sharp | $(PS2PDF) - - >gv.3sharp.pdf

gv.3go.pdf: gv.3go
	- $(GROFF) -Tps -man gv.3go | $(PS2PDF) - - >gv.3go.pdf

gv.3guile.pdf: gv.3guile
	- $(GROFF) -Tps -man gv.3guile | $(PS2PDF) - - >gv.3guile.pdf

gv.3io.pdf: gv.3io
	- $(GROFF) -Tps -man gv.3io | $(PS2PDF) - - >gv.3io.pdf

gv.3java.pdf: gv.3java
	- $(GROFF) -Tps -man gv.3java | $(PS2PDF) - - >gv.3java.pdf

gv.3lua.pdf: gv.3lua
	- $(GROFF) -Tps -man gv.3lua | $(PS2PDF) - - >gv.3lua.pdf

gv.3ocaml.pdf: gv.3ocaml
	- $(GROFF) -Tps -man gv.3ocaml | $(PS2PDF) - - >gv.3ocaml.pdf

gv.3perl.pdf: gv.3perl
	- $(GROFF) -Tps -man gv.3perl | $(PS2PDF) - - >gv.3perl.pdf

gv.3php.pdf: gv.3php
	- $(GROFF) -Tps -man gv.3php | $(PS2PDF) - - >gv.3php.pdf

gv.3python.pdf: gv.3python
	- $(GROFF) -Tps -man gv.3python | $(PS2PDF) - - >gv.3python.pdf

gv.3r.pdf: gv.3r
	- $(GROFF) -Tps -man gv.3r | $(PS2PDF) - - >gv.3r.pdf

gv.3ruby.pdf: gv.3ruby
	- $(GROFF) -Tps -man gv.3ruby | $(PS2PDF) - - >gv.3ruby.pdf

gv.3tcl.pdf: gv.3tcl
	- $(GROFF) -Tps -man gv.3tcl | $(PS2PDF) - - >gv.3tcl.pdf

install-data-hook:
if WITH_JAVA
	(cd $(DESTDIR)$(pkgjavadir); $(LN_S) -f org/graphviz/libgv_java.$(JSHEXT) ../../libgv.$(JSHEXT); javac -classpath $(DESTDIR)$(libjavadir) gv.java;)
endif
if WITH_LUA
	(cd $(DESTDIR)$(pkgluadir);    rm -f gv.so;  $(LN_S) libgv_lua.so gv.so;)
endif
if WITH_PERL
	(cd $(DESTDIR)$(pkgperldir);   rm -f gv.so;  $(LN_S) libgv_perl.so gv.so;)
endif
if WITH_PHP
	(cd $(DESTDIR)$(pkgphpdir);   rm -f gv.so;  $(LN_S) libgv_php.so gv.so;)
endif
if WITH_PYTHON
	(cd $(DESTDIR)$(pkgpythondir); rm -f _gv.so; $(LN_S) libgv_python.so _gv.so;)
endif
if WITH_PYTHON2
	(cd $(DESTDIR)$(pkgpython2dir); rm -f _gv.so; $(LN_S) libgv_python2.so _gv.so;)
endif
if WITH_PYTHON3
	(cd $(DESTDIR)$(pkgpython3dir); rm -f _gv.so; $(LN_S) libgv_python3.so _gv.so;)
endif
if WITH_R
	(cd $(DESTDIR)$(pkgRdir);   rm -f gv.so;  $(LN_S) libgv_R.so gv.so;)
endif
if WITH_RUBY
	(cd $(DESTDIR)$(pkgrubydir);   rm -f gv.so;  $(LN_S) libgv_ruby.so gv.so;)
endif

devtsts: $(DEVTSTS)

.PHONY: test_sharp
test_sharp: libgv_sharp.la
	-(mkdir -p test_sharp; cd test_sharp; \
		ln -fs ../.libs/libgv_sharp.so libgv_sharp.so; \
		ln -fs ../$(srcdir)/*.gv ../$(srcdir)/*.cs .; \
		$(SHARP) *.cs -out:test.exe; \
		mono test.exe)

.PHONY: test_go
test_go: libgv_go.la
	-(mkdir -p test_go; cd test_go; \
		ln -fs ../.libs/libgv_go.so libgv.so; \
		ln -fs ../$(srcdir)/*.gv ../$(srcdir)/*.go .; \
		$(GO) *.go
		LD_LIBRARY_PATH=. go test)

.PHONY: test_guile
test_guile: libgv_guile.la
	-(mkdir -p test_guile; cd test_guile; \
		ln -fs ../.libs/libgv_guile.so libgv.so; \
		ln -fs ../$(srcdir)/*.gv ../$(srcdir)/*.guile .; \
		$(GUILE) -s test.guile)

.PHONY: test_io
test_io: libgv_io.la
	-(mkdir -p test_io; cd test_io; \
		ln -fs ../.libs/libgv_io.so libgv.so; \
		ln -fs ../$(srcdir)/*.gv ../$(srcdir)/*.io .; \
		ioc *.io; \
		LD_LIBRARY_PATH=. io test)

.PHONY: test_java
test_java: libgv_java.la
	-(mkdir -p test_java/org/graphviz; cd test_java; \
		ln -f ../.libs/libgv_java.so libgv.jnilib; \
		ln -f ../$(srcdir)/*.java org/graphviz; \
		ln -f ../$(srcdir)/test.java .; \
		ln -f ../$(srcdir)/hello.gv  .; \
		ln -f ../$(srcdir)/test.java .; \
		CLASSPATH=. javac *.java; \
		LD_LIBRARY_PATH=. CLASSPATH=. java test)

.PHONY: test_lua
test_lua: libgv_lua.la
	-(mkdir -p test_lua; cd test_lua; \
		ln -fs ../.libs/libgv_lua.so gv.so;\
		ln -fs ../$(srcdir)/*.gv ../$(srcdir)/*.lua .; \
		$(LUA) test.lua)

.PHONY: test_ocaml
test_ocaml: libgv_ocaml.la
	-(mkdir -p test_ocaml; cd test_ocaml; \
		ln -fs ../.libs/libgv_ocaml.so libgv.so; \
		ln -fs ../$(srcdir)/*.gv ../$(srcdir)/*.ml ../$(srcdir)/*.mli .; \
		ocamlc *.ocaml; \
		LD_LIBRARY_PATH=. ocaml test)

.PHONY: test_perl
test_perl: libgv_perl.la
	-(mkdir -p test_perl; cd test_perl; \
		ln -fs ../.libs/libgv_perl.so gv.so; \
		ln -fs ../$(srcdir)/*.gv ../$(srcdir)/*.p[lm] .; \
		$(PERL) ./test.pl)

.PHONY: test_php
test_php: libgv_php.la
	-(mkdir -p test_php; cd test_php; \
		ln -fs ../.libs/libgv_php.so libgv.so; \
		ln -fs ../$(srcdir)/*.gv ../$(srcdir)/*.php .; \
		$(PHP) ./test.php)

.PHONY: test_python
test_python: libgv_python.la
	-(mkdir -p test_python; cd test_python; \
		ln -fs ../.libs/libgv_python.so _gv.so; \
		ln -fs ../$(srcdir)/*.gv ../$(srcdir)/*.py .; \
		PYTHONPATH=. $(PYTHON) test.py)

.PHONY: test_python2
test_python2: libgv_python2.la
	-(mkdir -p test_python2; cd test_python2; \
		ln -fs ../.libs/libgv_python2.so _gv.so; \
		ln -fs ../$(srcdir)/*.gv ../$(srcdir)/*.py .; \
		PYTHONPATH=. $(PYTHON2) test.py)

.PHONY: test_python3
test_python3: libgv_python3.la
	-(mkdir -p test_python3; cd test_python3; \
		ln -fs ../.libs/libgv_python3.so _gv.so; \
		ln -fs ../$(srcdir)/*.gv ../$(srcdir)/*.py .; \
		PYTHONPATH=. $(PYTHON3) test.py)

.PHONY: test_R
test_R: libgv_R.la
	-(mkdir -p test_R; cd test_R; \
		ln -fs ../.libs/libgv_R.so gv.so; \
		ln -fs ../$(srcdir)/*.gv ../$(srcdir)/*.R .; \
		$(R) test.R)

.PHONY: test_ruby
test_ruby: libgv_ruby.la
	-(mkdir -p test_ruby; cd test_ruby; \
		ln -fs ../.libs/libgv_ruby.so gv.so; \
		ln -fs ../$(srcdir)/*.gv ../$(srcdir)/*.rb .; \
		$(RUBY) test.rb)

.PHONY: test_tcl
test_tcl: libgv_tcl.la
	-(mkdir -p test_tcl; cd test_tcl; \
		ln -fs ../.libs/libgv_tcl.so libgv_tcl.so; \
		ln -fs ../$(srcdir)/*.gv ../$(srcdir)/*.tcl .; \
		$(TCLSH) ./test.tcl)

EXTRA_DIST = $(man) $(pdf) gv_doc_*.tcl

CLEANFILES = test_*/* pkgIndex.tcl \
	gv_sharp.cpp $(SHARP_data) \
	gv_go.cpp $(GO_data) \
	gv_guile.cpp $(GUILE_data) \
	gv_io.cpp $(IO_data) \
	gv_java.cpp $(JAVA_data) \
	gv_lua.cpp $(LUA_data) \
	gv_ocaml.cpp gv.cma gv.cmi gv.mli swig.mli swig.ml swig.cmi swig.cmo gv.ml gv.cmo gv.cmx gv.cmxa gv.a gv.o \
	gv_perl.cpp $(PERL_data) \
	gv_php.cpp $(PHP_data) \
	gv_python.cpp $(PYTHON_data) gv.pyc \
	gv_python2.cpp $(PYTHON2_data) gv.pyc \
	gv_python3.cpp $(PYTHON3_data) gv.pyc \
	gv_R.cpp $(R_data) \
	gv_ruby.cpp $(RUBY_data) \
	gv_tcl.cpp $(TCL_data) pkgIndex.tcl

DISTCLEANFILES = gv_*.n gv_*.pdf
