PYVERSION=3.5
PROJECT=pymongols$(shell python$(PYVERSION)-config --extension-suffix)
CPPSRC=$(shell find src -type f -name *.cpp)
CPPOBJ=$(patsubst %.cpp,%.o,$(CPPSRC))
CCSRC=$(shell find src -type f -name *.cc)
CCOBJ=$(patsubst %.cc,%.o,$(CCSRC))
CXXSRC=$(shell find src -type f -name *.cxx)
CXXOBJ=$(patsubst %.cxx,%.o,$(CXXSRC))

CSRC=$(shell find src -type f -name *.c)
COBJ=$(patsubst %.c,%.o,$(CSRC))

OBJ=$(COBJ) $(CXXOBJ) $(CCOBJ) $(CPPOBJ)

CC=gcc
CXX=g++

CFLAGS+=-O3 -std=c11 -Wall -fPIC `python$(PYVERSION)-config --cflags` `pkg-config --cflags mongols openssl`
CXXFLAGS+=-O3 -std=c++11 -Wall -fPIC `python$(PYVERSION)-config --cflags` `pkg-config --cflags mongols openssl`
LDLIBS+=`python$(PYVERSION)-config --libs` `pkg-config --libs mongols openssl`
LDFLAGS+=-shared


ifndef INSTALL_DIR
INSTALL_DIR=/usr/lib/python$(PYVERSION)/lib-dynload
endif


all:$(PROJECT)

$(PROJECT):$(OBJ)
	$(CXX) $(LDFLAGS) -o $@ $^ $(LDLIBS) 

.c.o:
	$(CC) $(CFLAGS) -c $< -o $@

.cpp.o:
	$(CXX) $(CXXFLAGS)  -c $< -o $@

.cc.o:
	$(CXX) $(CXXFLAGS)  -c $< -o $@
	
.cxx.o:
	$(CXX) $(CXXFLAGS)  -c $< -o $@

clean:
	@for i in $(OBJ);do echo "rm -f" $${i} && rm -f $${i} ;done
	rm -f $(PROJECT)

install:
	test -d $(INSTALL_DIR)/ || mkdir -p $(INSTALL_DIR)/
	install $(PROJECT) $(INSTALL_DIR)

