# EXE = $${PWD\#\#*/}
TEST = $(patsubst %.cpp, %, $(wildcard *test*.cpp nested.cpp))
EXE = $(strip $(patsubst %.cpp, %, $(filter-out $(TEST:=.cpp),$(wildcard p[0-9]*.cpp) Driver.cpp)))

GDBFLAGS = -O2 -ggdb
CXXFLAGS = -Og -ggdb
# CXXFLAGS = -Wall -g
# SOURCES = $(filter-out $(patsubst %, %.cpp,$(EXE)),$(wildcard *.cpp))
SOURCES = $(filter-out $(EXE:=.cpp) $(TEST:=.cpp),$(wildcard *.cpp))
SOURCES2 = $(SOURCES:.cpp=)
OBJS = $(SOURCES:.cpp=.o)
DEPS = $(wildcard *.h)

initalize: $(OBJS)

test: $(TEST)

all: $(EXE) run

build: $(EXE)

c: clean all

echo:
	echo TEST: $(TEST)
	echo EXE: $(EXE)
	echo SOURCES: $(SOURCES)
	echo SOURCES2: $(SOURCES2)
	echo OBJS: $(OBJS)
	echo DEPS: $(DEPS)

	
$(EXE): %: %.cpp $(OBJS)
	$(CXX) $(CXXFLAGS) $(filter-out $<,$(OBJS:.o=.cpp)) $< -o $@
	
$(TEST): %: %.cpp $(OBJS)
	$(CXX) $(CXXFLAGS) $(filter-out $<,$(OBJS:.o=.cpp)) $< -o $@
	./$@
%.o: %.cpp
	$(CXX) $(CXXFLAGS) -c $< 
	

%: %.cpp $(OBJS)
	$(CXX) $(CXXFLAGS) $(filter-out $<,$(OBJS:.o=.cpp)) $< -o $%

clean:
	rm -f $(EXE) $(TEST) $(OBJS) $(wildcard *.stackdump) $(wildcard *.exe)
	# rm *.o*
	# rm *~ 

run(%): %.cpp $(OBJS)
	$(CXX) $(CXXFLAGS) $(filter-out $<,$(OBJS:.o=.cpp)) $< -o $%
	./$% < input.txt > output.txt
	
gdb(%): %.cpp $(OBJS) 
	$(CXX) $(CXXFLAGS) $(filter-out $<,$(OBJS:.o=.cpp)) $< -o $%
	./$@.bat $% 	

gdb2(%): %.cpp $(OBJS) 
	$(CXX) $(GDBFLAGS) $(filter-out $<,$(OBJS:.o=.cpp)) $< -o $%
	./gdb.bat $% 	

diff(%): %.txt run(%)
	diff $< output.txt
	# type diff.txt
	
run:
	./$(EXE)


	
depend: .depend

.depend: $(DEPS)
	rm -f ./.depend
	$(CC) $(CXXFLAGS) -MM $^ > ./.depend;
	
include .depend


