################################################################################
#
# Makefile to compile and link C programs
#
# Version valid for Linux machines
#
# "make" compiles and links the specified main programs and modules
# using the specified libraries (if any), and produces the executables
# 
# "make clean" removes all files created by "make"
#
# Dependencies on included files are automatically taken care of
#
################################################################################

all: rmxeq mkdep mkxeq
.PHONY: all

# Local directories
SOURCEDIR=src
INCLUDEDIR=inc
RESDIR=res
DESTDIR=.

# Add sourcedir to vpath
VPATH=$(SOURCEDIR)

# MC sample directory
#SAMPLEDIR= -DSAMPLEDIR=\"/data/atlas/atlasdata2/DiHiggsSharedSamples/\"
#SAMPLEDIR= -DSAMPLEDIR=\"/mnt/lustre/hartland/OxfordHH4bSamples/\"
SAMPLEDIR= -DSAMPLEDIR=\"/mnt/greenraid/scratch2/HJT/\"
# Pythia 8 xmldoc directory
PYTHIADIR = -DPYTHIADIR=\"$(shell pythia8-config --xmldoc)\"

MACROS = $(SAMPLEDIR) $(PYTHIADIR) -DRESDIR=\"$(RESDIR)\"

# Compiler options
##################

GCC = g++

# hepmc2 
HEPMCLIBS = -lHepMC -L /afs/cern.ch/sw/lcg/external/HepMC/2.06.08/x86_64-slc6-gcc47-opt/lib
HEPMCCFLAGS = -I /afs/cern.ch/sw/lcg/external/HepMC/2.06.08/x86_64-slc6-gcc47-opt/include

# fastjet 
FASTJETLIBS = -L /users/hannahtillim/libs/fastjet/lib -lfastjettools -lSoftKiller -lfastjet
FASTJETCFLAGS = -I /users/hannahtillim/libs/fastjet/include -I /users/hannahtillim/libs/fjcontrib/include

# fastjet-contrib
FJCONTRIB = -L /users/hannahtillim/libs/fjcontrib/lib -lVariableR -lEnergyCorrelator -lNsubjettiness

# Pythia 8 
PYTHIALIBS = $(shell pythia8-config --libs) -ldl
PYTHIACFLAGS = $(shell pythia8-config --cxxflags)

# YODA
YODALIBS = $(shell yoda-config --libs)
YODACFLAGS = $(shell yoda-config --cxxflags)

CFLAGS = -std=c++11 -O3 -Wno-unknown-pragmas -I /usr/local/include -I $(INCLUDEDIR) $(FASTJETCFLAGS) $(PYTHIACFLAGS) $(YODACFLAGS) $(HEPMCCFLAGS) $(MACROS)
LIBS = $(FASTJETLIBS) $(PYTHIALIBS) $(YODALIBS) $(FJCONTRIB) $(HEPMCLIBS)

INCPATH = $(CFLAGS)

#####################################

# main programs and required modules 

MAIN =  HHH6b

MODULES = samples sherpa-xs analysis detector run utils triHiggsAnalysis

############################## do not change ###################################

SHELL=/bin/bash

CC=$(GCC)

PGMS= $(MAIN) $(MODULES)

INCDIRS = $(INCPATH)

OBJECTS = $(addsuffix .o,$(MODULES))

LDFLAGS = $(LIBS)

-include $(addsuffix .d,$(PGMS))

# rule to make dependencies
$(addsuffix .d,$(PGMS)): %.d: %.cc Makefile
	@ $(CC) -MM -ansi $(INCDIRS) $< -o $@

# rule to compile source programs
$(addsuffix .o,$(PGMS)): %.o: %.cc Makefile
	$(CC) $< -c $(CFLAGS) $(INCDIRS) -o $@

# rule to link object files
$(MAIN): %: %.o $(OBJECTS) Makefile
	$(CC) $< $(OBJECTS) $(CFLAGS) $(LDFLAGS) -o $@

# produce executables
mkxeq: $(MAIN)

# remove old executables and old error log file
rmxeq:
	@ -rm -f $(MAIN); \
        echo "delete old executables"		

# make dependencies
mkdep:  $(addsuffix .d,$(PGMS))
	@ echo "generate tables of dependencies"

# clean directory 
clean:
	@ -rm -rf *.d *.o *~ $(MAIN)

.PHONY: clean

################################################################################


