# ----- If the user defined OPENBLAS on the command line or use that
ifdef OPENBLAS
$(info $$OPENBLAS is [${OPENBLAS}])
else
# ----- If this is Broad, check the path to see if "use .openblas-0.2.8"
ifeq ($(DOMAINNAME),broadinstitute.org)
$(info *** Broad Institute users can execute use .openblas-0.2.8 and use GCC-4.9 to link to OpenBLAS ***)
else
# ----- On orchestra this will work  
OPENBLAS=/opt/openblas
$(info setting $$OPENBLAS to [${OPENBLAS}])
endif
endif

ifdef OPENBLAS
CFLAGS  = -I../include -I$(OPENBLAS)/include -D_GNU_SOURCE
LDFLAGS = -L$(OPENBLAS)/lib -D_GNU_SOURCE
LDLIBS  = -Wl,-Bdynamic -lgsl -Wl,-Bstatic -lopenblas -Wl,-Bdynamic -lgfortran -pthread -lm
else
CFLAGS  = -I../include -D_GNU_SOURCE
LDFLAGS = -D_GNU_SOURCE
LDLIBS  = -lgsl -lopenblas -lgfortran -pthread -lm
endif

CC = gcc
LD = ld


ifeq ($(OPTIMIZE), 1)
	CFLAGS += -O2
endif

ifeq ($(DEBUG), 1)
	CFLAGS += -g # enable debugging
endif

ifeq ($(PROFILING), 1)
	CFLAGS += -pg # enable profiling
endif

ND=nicksrc
ED=eigensrc
KG=ksrc
NLIB = $(ND)/libnick.a

# ----- phony targets
.PHONY: all clean clobber install

EXE = baseprog convertf mergeit pca \
	$(ED)/pcatoy $(ED)/smartrel $(ED)/smarteigenstrat \
	$(ED)/twstats $(ED)/eigenstrat $(ED)/eigenstratQTL $(ED)/smartpca

all:    $(EXE)

install:  all
	mv $(EXE) ../bin

clobber:
	rm -f *.o */*.o */*.a
	rm -f $(EXE)
	cd ../bin/ ; rm -f $(notdir, $(EXE)) ; cd ../src

clean:
	rm -f *.o core core.* *.o  

# ----- build nicksrc/libnick.a
$(NLIB):
	$(MAKE) -C $(ND)

baseprog:	baseprog.o mcio.o egsubs.o admutils.o h2d.o $(ED)/exclude.o $(NLIB)

convertf:	convertf.o mcio.o egsubs.o admutils.o h2d.o $(ED)/exclude.o $(NLIB)

mergeit:	mergeit.o mcio.o admutils.o $(NLIB)

pca:	pca.o $(ED)/eigsubs.o eigx.o $(NLIB)

$(ED)/pcatoy: 	$(ED)/pcatoy.o eigensrc/eigsubs.o eigensrc/eigx.o $(NLIB)

$(ED)/smartrel:	$(ED)/smartrel.o twsubs.o mcio.o qpsubs.o admutils.o egsubs.o regsubs.o \
		$(ED)/eigsubs.o $(ED)/eigx.o $(ED)/smartsubs.o $(NLIB)    

$(ED)/smarteigenstrat:	$(ED)/smarteigenstrat.o mcio.o admutils.o $(NLIB)

$(ED)/twstats:	$(ED)/twstats.o $(NLIB) 

#$(ED)/eigenstrat:	$(ED)/eigenstrat.o 

#$(ED)/eigenstratQTL:	$(ED)/eigenstratQTL.o 

$(ED)/smartpca:	$(ED)/smartpca.o $(ED)/eigsubs.o $(ED)/exclude.o $(ED)/smartsubs.o $(ED)/eigx.o \
		twsubs.o mcio.o qpsubs.o admutils.o egsubs.o regsubs.o gval.o \
		$(NLIB) \
          	$(KG)/kjg_fpca.o $(KG)/kjg_gsl.o

smartpca: $(ED)/smartpca
	cp $< $@

$(ED)/fffpca:	$(ED)/fffpca.o $(ED)/eigsubs.o $(ED)/exclude.o $(ED)/smartsubs.o $(ED)/eigx.o \
		twsubs.o mcio.o qpsubs.o admutils.o egsubs.o regsubs.o gval.o \
		$(NLIB) \
          	$(KG)/kjg_fpca.o $(KG)/kjg_gsl.o

fffpca: $(ED)/fffpca
	cp $< $@
