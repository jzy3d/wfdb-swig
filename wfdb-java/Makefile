# file: Makefile	G. Moody		12 July 2011

WFDB_HOME=/usr/local
WFDB_INCLUDE=$(WFDB_HOME)/include

# Uncomment one of the next two lines:
JAVAC=javac
# JAVAC=gcj

all:
	rm -rf build
	cp -pr conf build
	cp -p ../AUTHORS ../ChangeLog ../COPYING ../INSTALL ../NEWS ../README \
	  build
	swig -I$(WFDB_HOME)/include -java -package wfdb -outdir build/wfdb \
	  -o build/wfdb_java_wrap.c ../wfdb.i
	cd build; autoreconf -i
	cd build; ./acgen.sh
	cd build; ./configure JAVAC=$(JAVAC) --build=$(TARGET) --prefix=$(WFDB_HOME) CFLAGS="-O1 -I$(WFDB_INCLUDE) $(CFLAGS)"
	cd build; $(MAKE)

install:
	cd build; $(MAKE) install

check:
	cp ../examples/*.java examples
	cd examples; $(MAKE) JAVAC=$(JAVAC)
	@if [ -s examples/psamples ]; then examples/psamples >check.out; \
	 else examples/jw examples/psamples >check.out; fi
	@if ( diff -q check.out ../examples/psamples.out ) \
	 then echo "Passed"; else echo "Failed"; fi

rpm:
	rm -rf build wfdb-java wfdb-java.tar.gz
	$(MAKE) all
	mv build wfdb-java
	tar cfvz wfdb-java.tar.gz wfdb-java/
	rpmbuild -ta wfdb-java.tar.gz
	mv wfdb-java build

clean:
	cd examples; $(MAKE) clean
	rm -rf build check.out examples/*.java wfdb-java.tar.gz *~


