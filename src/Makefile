CC = gcc
CFLAGS = -Wall -Werror -Wextra -std=c11
TEST_LIBS = -lcheck -lm
OBJECTS=s21_math.o
SOURSES=s21_math.c


all: clean s21_math.a test gcov_report

s21_math.a: s21_math.o
	$(CC) -c $(SOURSES)
	ar rcs s21_math.a $(OBJECTS)

clean:
	rm -rf ./*.o ./*.a ./a.out gcov_test *.html *.css ./GcovReport ./*.gcno ./*.gcda ./report ./*.info ./math_o ./*.dSYM ./test_s21math

test: test_s21math.c s21_math.a
	$(CC) $(CFLAGS) -c test_s21math.c -o test.o
	$(CC) test.o s21_math.a $(TEST_LIBS) -o test

gcov_report: s21_math.a 
	$(CC) $(CFLAGS) --coverage test_s21math.c s21_math.c s21_math.a $(TEST_LIBS) -o gcov_test
	chmod +x *
	./gcov_test
	lcov -t "gcov_test" -o gcov_test.info --no-external -c -d .
	genhtml -o report/ gcov_test.info
	open ./report/index.html

check: test
	cppcheck --enable=all --suppress=missingIncludeSystem --inconclusive --check-config *.c *.h
	cp ../materials/linters/.clang-format .clang-format
	clang-format -style=Google *.h *.c -i
	rm -rf .clang-format
	make test
	leaks --atExit -- test


rebuild: clean all
	open ./report/index.html
