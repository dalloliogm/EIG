CFLAGS  = -I../include -I/opt/openblas/include
LDFLAGS = -L/opt/openblas/lib -static
LDLIBS  = -lgsl -lopenblas -lgfortran -pthread -lm -lrt

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
	$(ED)/pcatoy $(ED)/oldsmartpca $(ED)/smartrel $(ED)/smarteigenstrat \
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

$(ED)/oldsmartpca:	$(ED)/oldsmartpca.o twsubs.o mcio.o qpsubs.o admutils.o egsubs.o regsubs.o \
		$(ED)/eigsubs.o  \
		$(ED)/exclude.o $(ED)/smartsubs.o $(ED)/eigx.o $(NLIB)

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

smartpca:	$(ED)/smartpca
		cp $(ED)/smartpca smartpca
