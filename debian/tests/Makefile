# -*- makefile -*-

# Some info about autopkgtest environment:
# AUTOPKGTEST_BASEDIR: Dir created with mktemp. NOT AVAILABLE AS ENVVAR.
# AUTOPKGTEST_TMP: AUTOPKGTEST_BASEDIR/autopkgtest_tmp
# $0: AUTOPKGTEST_BASEDIR/tree/debian/tests/${TEST_PROGRAM}
# PWD: AUTOPKGTEST_BASEDIR/tree    # autopkg starts at the unpacked source tree

all: sanity-check filter-check wide_test_valid wide_test_invalid wide_test_invalid-but_ok cxx_warnings_test

wide_test_valid: wide_test_valid.c
	$(CXX) $< -laspell -o $@
	./$@

wide_test_invalid: wide_test_invalid.c
	$(CXX) $< -laspell -o $@
	./$@ 2> $@.log || true
	grep -F -q 'Null-terminated wide-character strings unsupported when used this way.' "$@.log"

wide_test_invalid-but_ok: wide_test_invalid.c
	$(CXX) -DASPELL_ENCODE_SETTING_SECURE $< -laspell -o $@
	./$@

cxx_warnings_test: cxx_warnings_test.cpp
	$(CXX) -Wall -Wconversion -Werror -c $<

clean:
	rm -f wide_test_invalid.log cxx_warnings_test.o wide_test_invalid-but_ok wide_test_invalid wide_test_valid
	- ( cd tmp && rm -f * ); rmdir tmp
