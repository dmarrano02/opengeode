include ../shared.mk

all: test-ada test-llvm

edit:
	$(OPENGEODE) expressions.pr system_structure.pr

test-parse:
	$(OPENGEODE) expressions.pr system_structure.pr --check

test-ada: expressions.ali dataview-uniq.o | test_ada.o
	$(GNATBIND) -n expressions.ali
	$(GNATLINK) test_ada.o expressions.ali -lgnat -lm -o test_ada
	./test_ada

test-c: expressions.c test_c.o dataview-uniq.o
	$(CC) expressions.c -c -o expressions.o
	$(CC) test_c.o dataview-uniq.o expressions.o -lm -o test_c
	./test_c

test-llvm: expressions.o dataview-uniq.o | test_llvm.o
	$(CC) expressions.o dataview-uniq.o test_llvm.o -o test_llvm -lm
	./test_llvm

test-vhdl : test-c
	bambu expressions.c test_c.c dataview-uniq.c --experimental-setup=BAMBU -v4

coverage:
	coverage run -p $(OPENGEODE) expressions.pr system_structure.pr --toAda

.PHONY: all edit test-parse test-ada test-llvm coverage
