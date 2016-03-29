cmake_minimum_required(VERSION 2.8)
include(noweb.cmake)
include(UseLatex)
set(libtest_C
    test-suite.c)
set(libtest_H
    test-suite.h)
set(libtest_TEX
    test-suite.tex)

tangle("${libtest_H}")
tangle("${libtest_C}")
weave("${libtest_TEX}")
add_custom_target(tangle DEPENDS ${libtest_H} ${libtest_C})
add_custom_target(weave DEPENDS ${libtest_TEX})

add_library(test SHARED ${libtest_C})
add_dependencies(test tangle)

add_latex_document(libtest.tex
                   INPUTS preamble.tex
                   FORCE_PDF
                   TARGET_NAME libtest)

add_dependencies(libtest_pdf weave)
