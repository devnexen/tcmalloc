# Copyright 2019 The TCMalloc Authors
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     https://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

# -*- mode: python; -*-

# Description:
# Tests for tcmalloc, including a performance test.

load("//tcmalloc:copts.bzl", "TCMALLOC_DEFAULT_COPTS")
load("//tcmalloc:variants.bzl", "create_tcmalloc_testsuite")

licenses(["notice"])  # Apache 2.0

REGTEST_DEPS = [
    ":testutil",
    ":thread_manager",
    "//tcmalloc:headers_for_tests",
    "@com_google_googletest//:gtest",
    "@com_google_absl//absl/base",
    "@com_google_absl//absl/base:core_headers",
    "//tcmalloc/internal:logging",
    "//tcmalloc/internal:parameter_accessors",
    "@com_google_absl//absl/random",
    "@com_google_absl//absl/random:distributions",
    "@com_google_absl//absl/strings",
    "@com_google_absl//absl/strings:str_format",
    "@com_google_absl//absl/synchronization",
    "@com_google_absl//absl/utility",
    "//tcmalloc/internal:declarations",
    "//tcmalloc:malloc_extension",
]

REGTEST_OPTS = [
    "$(STACK_FRAME_UNLIMITED)",  # required for tcmalloc_test.cc
] + TCMALLOC_DEFAULT_COPTS

EMPIRICAL_DEPS = [
    ":empirical",
    ":empirical_distributions",
    ":testutil",
    "//tcmalloc/internal:atomic_stats_counter",
    "//tcmalloc/internal:logging",
    "//tcmalloc/internal:util",
    "//tcmalloc:want_hpaa",
    "@com_google_absl//absl/base",
    "@com_google_absl//absl/base:core_headers",
    "@com_google_absl//absl/flags:flag",
    "@com_google_absl//absl/flags:parse",
    "@com_google_absl//absl/memory",
    "@com_google_absl//absl/random:distributions",
    "@com_google_absl//absl/strings",
    "@com_google_absl//absl/strings:str_format",
    "@com_google_absl//absl/synchronization",
    "@com_google_absl//absl/time",
    "//tcmalloc:malloc_extension",
]

cc_library(
    name = "testutil",
    testonly = 1,
    srcs = ["testutil.cc"],
    hdrs = ["testutil.h"],
    copts = TCMALLOC_DEFAULT_COPTS,
    visibility = [
        "//tcmalloc:__subpackages__",
    ],
    deps = [
        "//tcmalloc:malloc_extension",
        "@com_google_absl//absl/base:core_headers",
    ],
)

create_tcmalloc_testsuite(
    name = "tcmalloc_test",
    timeout = "long",
    srcs = ["tcmalloc_test.cc"],
    copts = REGTEST_OPTS,
    linkstatic = 1,  # get the most realistic performance
    tags = [
        "noasan",
        "nomsan",
        "notsan",
    ],
    visibility = ["//visibility:private"],
    deps = REGTEST_DEPS + ["@com_github_google_benchmark//:benchmark"],
)

cc_binary(
    name = "tcmalloc_benchmark",
    testonly = 1,
    srcs = ["tcmalloc_benchmark.cc"],
    copts = TCMALLOC_DEFAULT_COPTS,
    linkstatic = 1,
    malloc = "//tcmalloc",
    visibility = [
        "//visibility:private",
    ],
    deps = [
        "//tcmalloc:malloc_extension",
        "//tcmalloc/internal:declarations",
        "@com_github_google_benchmark//:benchmark_main",
        "@com_google_absl//absl/random",
    ],
)

cc_test(
    name = "tcmalloc_test_no_hp_aware",
    srcs = ["tcmalloc_test.cc"],
    copts = REGTEST_OPTS,
    linkstatic = 1,  # get the most realistic performance
    malloc = "//tcmalloc",
    tags = [
        "noasan",
        "nomsan",
        "notsan",
    ],
    deps = REGTEST_DEPS + [
        "@com_github_google_benchmark//:benchmark",
        "//tcmalloc:want_no_hpaa",
    ],
)

cc_test(
    name = "tcmalloc_test_hp_aware",
    srcs = ["tcmalloc_test.cc"],
    copts = REGTEST_OPTS,
    linkstatic = 1,  # get the most realistic performance
    malloc = "//tcmalloc",
    tags = [
        "noasan",
        "nomsan",
        "notsan",
    ],
    deps = REGTEST_DEPS + [
        "@com_github_google_benchmark//:benchmark",
        "//tcmalloc:want_hpaa",
    ],
)

cc_test(
    name = "tcmalloc_test_deprecated_perthread",
    timeout = "long",
    srcs = ["tcmalloc_test.cc"],
    copts = REGTEST_OPTS + ["-DTCMALLOC_DEPRECATED_PERTHREAD"],
    linkstatic = 1,  # get the most realistic performance
    malloc = "//tcmalloc:tcmalloc_deprecated_perthread",
    tags = [
        "noasan",
        "nomsan",
        "notsan",
    ],
    deps = REGTEST_DEPS + ["@com_github_google_benchmark//:benchmark"],
)

create_tcmalloc_testsuite(
    name = "threadcachesize_test",
    srcs = ["threadcachesize_test.cc"],
    copts = TCMALLOC_DEFAULT_COPTS,
    deps = [
        ":testutil",
        "//tcmalloc:malloc_extension",
        "@com_github_google_benchmark//:benchmark",
        "@com_google_absl//absl/base:core_headers",
        "@com_google_absl//absl/synchronization",
        "@com_google_googletest//:gtest_main",
    ],
)

cc_test(
    name = "markidle_test",
    srcs = ["markidle_test.cc"],
    copts = TCMALLOC_DEFAULT_COPTS,
    malloc = "//tcmalloc:tcmalloc_deprecated_perthread",
    tags = [
        "noasan",
        "nomsan",
        "notsan",
    ],
    deps = [
        "//tcmalloc:malloc_extension",
        "@com_github_google_benchmark//:benchmark",
        "@com_google_absl//absl/flags:flag",
        "@com_google_googletest//:gtest_main",
    ],
)

cc_test(
    name = "memalign_test",
    srcs = ["memalign_test.cc"],
    copts = TCMALLOC_DEFAULT_COPTS,
    malloc = "//tcmalloc",
    tags = [
        "noasan",
        "nomsan",
        "notsan",
    ],
    deps = [
        ":testutil",
        "@com_github_google_benchmark//:benchmark",
        "@com_google_absl//absl/random",
        "@com_google_absl//absl/random:distributions",
        "@com_google_googletest//:gtest_main",
    ],
)

# Fragmentation timing and test
cc_test(
    name = "frag_test",
    srcs = ["frag_test.cc"],
    copts = TCMALLOC_DEFAULT_COPTS,
    malloc = "//tcmalloc",
    tags = [
        "noasan",
        "nomsan",
        "notsan",
    ],
    deps = [
        "//tcmalloc",
        "//tcmalloc:headers_for_tests",
        "//tcmalloc:malloc_extension",
        "@com_github_google_benchmark//:benchmark",
        "@com_google_googletest//:gtest_main",
    ],
)

create_tcmalloc_testsuite(
    name = "largesmall_frag_test",
    srcs = ["largesmall_frag_test.cc"],
    copts = [
        "$(STACK_FRAME_UNLIMITED)",  # largesmall_frag_test.cc
    ] + TCMALLOC_DEFAULT_COPTS,
    tags = [
        "noasan",
        "nomsan",
    ],
    deps = [
        ":testutil",
        "//tcmalloc:malloc_extension",
        "//tcmalloc/internal:linked_list",
        "//tcmalloc/internal:memory_stats",
        "@com_github_google_benchmark//:benchmark",
        "@com_google_absl//absl/base:core_headers",
        "@com_google_absl//absl/strings:str_format",
        "@com_google_googletest//:gtest_main",
    ],
)

cc_library(
    name = "thread_manager",
    testonly = 1,
    hdrs = ["thread_manager.h"],
    visibility = ["//tcmalloc:__subpackages__"],
    deps = [
        "@com_google_absl//absl/synchronization",
        "@com_google_googletest//:gtest",
    ],
)

cc_test(
    name = "outofmemory_test",
    srcs = ["outofmemory_test.cc"],
    copts = TCMALLOC_DEFAULT_COPTS,
    malloc = "//tcmalloc",
    tags = [
        "noasan",
        "nomsan",
        "notsan",
    ],
    deps = [
        ":testutil",
        "@com_github_google_benchmark//:benchmark",
        "@com_google_googletest//:gtest_main",
    ],
)

cc_test(
    name = "current_allocated_bytes_test",
    size = "small",
    srcs = ["current_allocated_bytes_test.cc"],
    copts = TCMALLOC_DEFAULT_COPTS,
    malloc = "//tcmalloc",
    deps = [
        "//tcmalloc:malloc_extension",
        "//tcmalloc/internal:logging",
        "@com_github_google_benchmark//:benchmark",
        "@com_google_absl//absl/base",
    ],
)

cc_test(
    name = "sampling_test",
    srcs = ["sampling_test.cc"],
    copts = TCMALLOC_DEFAULT_COPTS,
    linkstatic = 1,
    malloc = "//tcmalloc",
    tags = [
        "noasan",
        "nomsan",
        "notsan",
    ],
    deps = [
        ":testutil",
        "//tcmalloc:malloc_extension",
        "//tcmalloc/internal:logging",
        "@com_github_google_benchmark//:benchmark",
        "@com_google_absl//absl/base:core_headers",
        "@com_google_absl//absl/debugging:symbolize",
        "@com_google_absl//absl/strings",
        "@com_google_absl//absl/types:optional",
        "@com_google_googletest//:gtest_main",
    ],
)

create_tcmalloc_testsuite(
    name = "sampling_memusage_test",
    timeout = "long",
    srcs = ["sampling_memusage_test.cc"],
    copts = TCMALLOC_DEFAULT_COPTS,
    tags = [
        "noasan",
        "nomsan",
        "notsan",
    ],
    deps = [
        "//tcmalloc:headers_for_tests",
        "//tcmalloc:malloc_extension",
        "//tcmalloc/internal:logging",
        "//tcmalloc/internal:util",
        "@com_github_google_benchmark//:benchmark",
        "@com_google_absl//absl/base",
        "@com_google_absl//absl/strings",
        "@com_google_absl//absl/types:optional",
        "@com_google_googletest//:gtest",
    ],
)

# Test of sampler class
cc_test(
    name = "sampler_test",
    srcs = ["sampler_test.cc"],
    copts = TCMALLOC_DEFAULT_COPTS,
    linkstatic = 1,
    malloc = "//tcmalloc",
    tags = [
        "noasan",
        "nomsan",
        "notsan",
    ],
    deps = [
        ":testutil",
        "//tcmalloc",
        "//tcmalloc:headers_for_tests",
        "//tcmalloc:malloc_extension",
        "@com_github_google_benchmark//:benchmark",
        "@com_google_absl//absl/base:core_headers",
        "@com_google_absl//absl/strings",
        "@com_google_googletest//:gtest_main",
    ],
)

# Check amount of memory allocated at startup.
cc_test(
    name = "startup_size_test",
    srcs = ["startup_size_test.cc"],
    copts = TCMALLOC_DEFAULT_COPTS,
    linkstatic = 1,
    malloc = "//tcmalloc",
    tags = [
        "noasan",
        "nomsan",
        "notsan",
    ],
    deps = [
        "//tcmalloc:malloc_extension",
        "//tcmalloc/internal:logging",
        "@com_github_google_benchmark//:benchmark",
        "@com_google_absl//absl/base",
        "@com_google_googletest//:gtest_main",
    ],
)

cc_test(
    name = "large_alloc_size_test",
    srcs = ["large_alloc_size_test.cc"],
    copts = TCMALLOC_DEFAULT_COPTS,
    linkstatic = 1,
    malloc = "//tcmalloc",
    deps = [
        "//tcmalloc:headers_for_tests",
        "//tcmalloc:malloc_extension",
        "@com_github_google_benchmark//:benchmark",
        "@com_google_googletest//:gtest_main",
    ],
)

# Test for b/3485510.  Threads which heavily use malloc before TCMalloc's
# static initializers are invoked caused a race and crashes before cl/19792144
# fixed that race.
cc_library(
    name = "thread_ctor_test_lib",
    testonly = 1,
    srcs = ["thread_ctor_test_lib.cc"],
    copts = TCMALLOC_DEFAULT_COPTS,
    linkopts = ["-pthread"],
)

cc_test(
    name = "thread_ctor_test",
    srcs = ["thread_ctor_test.cc"],
    copts = TCMALLOC_DEFAULT_COPTS,
    malloc = "//tcmalloc",
    deps = [
        ":thread_ctor_test_lib",
        "@com_github_google_benchmark//:benchmark",
    ],
)

cc_test(
    name = "sample_size_class_test",
    srcs = ["sample_size_class_test.cc"],
    copts = TCMALLOC_DEFAULT_COPTS,
    linkstatic = 1,
    malloc = "//tcmalloc",
    tags = [
        "noasan",
        "nomsan",
        "notsan",
    ],
    deps = [
        "//tcmalloc",
        "//tcmalloc:malloc_extension",
        "//tcmalloc/internal:linked_list",
        "//tcmalloc/internal:logging",
        "@com_github_google_benchmark//:benchmark",
        "@com_google_googletest//:gtest_main",
    ],
)

cc_test(
    name = "no_deps_test",
    srcs = ["no_deps_test.cc"],
    copts = TCMALLOC_DEFAULT_COPTS,
    linkstatic = 1,
    malloc = "//tcmalloc",
    deps = ["@com_github_google_benchmark//:benchmark"],
)

cc_library(
    name = "empirical",
    testonly = 1,
    srcs = ["empirical.cc"],
    hdrs = ["empirical.h"],
    copts = TCMALLOC_DEFAULT_COPTS,
    deps = [
        "//tcmalloc/internal:logging",
        "@com_google_absl//absl/algorithm:container",
        "@com_google_absl//absl/functional:function_ref",
        "@com_google_absl//absl/random",
        "@com_google_absl//absl/random:distributions",
        "@com_google_absl//absl/random:seed_sequences",
        "@com_google_absl//absl/types:span",
    ],
)

cc_binary(
    name = "empirical_benchmark",
    testonly = 1,
    srcs = ["empirical_benchmark.cc"],
    copts = TCMALLOC_DEFAULT_COPTS,
    linkstatic = 1,
    malloc = "//tcmalloc",
    deps = [
        ":empirical",
        ":testutil",
        "@com_github_google_benchmark//:benchmark_main",
    ],
)

cc_test(
    name = "empirical_test",
    srcs = ["empirical_test.cc"],
    copts = TCMALLOC_DEFAULT_COPTS,
    linkstatic = 1,
    malloc = "//tcmalloc",
    deps = [
        ":empirical",
        ":testutil",
        "@com_github_google_benchmark//:benchmark",
        "@com_google_absl//absl/base:core_headers",
        "@com_google_absl//absl/random",
        "@com_google_absl//absl/random:distributions",
        "@com_google_absl//absl/strings",
        "@com_google_googletest//:gtest_main",
    ],
)

cc_library(
    name = "empirical_distributions",
    testonly = 1,
    srcs = ["empirical_distributions.cc"],
    hdrs = ["empirical_distributions.h"],
    copts = TCMALLOC_DEFAULT_COPTS,
    deps = [
        ":empirical",
        "@com_google_absl//absl/types:span",
    ],
)

cc_binary(
    name = "empirical_distributions_benchmark",
    testonly = 1,
    srcs = ["empirical_distributions_benchmark.cc"],
    malloc = "//tcmalloc",
    deps = [
        ":empirical_distributions",
        ":testutil",
        "@com_github_google_benchmark//:benchmark_main",
        "@com_google_absl//absl/base:core_headers",
        "@com_google_absl//absl/random",
        "@com_google_absl//absl/random:distributions",
        "@com_google_absl//absl/strings",
    ],
)

cc_test(
    name = "empirical_distributions_test",
    srcs = ["empirical_distributions_test.cc"],
    deps = [
        ":empirical_distributions",
        "@com_github_google_benchmark//:benchmark",
        "@com_google_absl//absl/base:core_headers",
        "@com_google_absl//absl/random",
        "@com_google_absl//absl/strings",
        "@com_google_absl//absl/strings:str_format",
        "@com_google_googletest//:gtest_main",
    ],
)

cc_binary(
    name = "empirical_driver",
    testonly = 1,
    srcs = ["empirical_driver.cc"],
    copts = TCMALLOC_DEFAULT_COPTS,
    linkstatic = 1,
    malloc = "//tcmalloc",
    deps = EMPIRICAL_DEPS,
)

cc_binary(
    name = "empirical_driver_hpaa",
    testonly = 1,
    srcs = ["empirical_driver.cc"],
    copts = TCMALLOC_DEFAULT_COPTS,
    linkstatic = 1,
    malloc = "//tcmalloc",
    deps = EMPIRICAL_DEPS,
)

cc_binary(
    name = "empirical_driver_small_but_slow",
    testonly = 1,
    srcs = ["empirical_driver.cc"],
    copts = TCMALLOC_DEFAULT_COPTS,
    linkstatic = 1,
    malloc = "//tcmalloc:tcmalloc_small_but_slow",
    deps = EMPIRICAL_DEPS,
)

cc_binary(
    name = "empirical_driver_256k_pages",
    testonly = 1,
    srcs = ["empirical_driver.cc"],
    copts = TCMALLOC_DEFAULT_COPTS,
    linkstatic = 1,
    malloc = "//tcmalloc:tcmalloc_256k_pages",
    deps = EMPIRICAL_DEPS,
)

cc_test(
    name = "limit_test",
    srcs = ["limit_test.cc"],
    copts = TCMALLOC_DEFAULT_COPTS,
    linkstatic = 1,
    malloc = "//tcmalloc",
    tags = [
        "noasan",
        "nomsan",
        "notsan",
    ],
    deps = [
        ":testutil",
        "//tcmalloc:headers_for_tests",
        "//tcmalloc:malloc_extension",
        "//tcmalloc:want_no_hpaa",
        "//tcmalloc/internal:logging",
        "//tcmalloc/internal:parameter_accessors",
        "@com_github_google_benchmark//:benchmark",
        "@com_google_absl//absl/base:core_headers",
        "@com_google_absl//absl/flags:flag",
        "@com_google_absl//absl/strings",
        "@com_google_absl//absl/strings:str_format",
        "@com_google_googletest//:gtest_main",
    ],
)

cc_test(
    name = "limit_test_hpaa",
    srcs = ["limit_test.cc"],
    copts = TCMALLOC_DEFAULT_COPTS,
    linkstatic = 1,
    malloc = "//tcmalloc",
    tags = [
        "noasan",
        "nomsan",
        "notsan",
    ],
    deps = [
        ":testutil",
        "//tcmalloc:headers_for_tests",
        "//tcmalloc:malloc_extension",
        "//tcmalloc:want_hpaa",
        "//tcmalloc/internal:logging",
        "//tcmalloc/internal:parameter_accessors",
        "@com_github_google_benchmark//:benchmark",
        "@com_google_absl//absl/base:core_headers",
        "@com_google_absl//absl/flags:flag",
        "@com_google_absl//absl/strings",
        "@com_google_absl//absl/strings:str_format",
        "@com_google_googletest//:gtest_main",
    ],
)

cc_test(
    name = "aligned_new_test",
    srcs = ["aligned_new_test.cc"],
    copts = TCMALLOC_DEFAULT_COPTS + ["-faligned-new"],
    linkstatic = 1,
    malloc = "//tcmalloc",
    tags = [
        "noasan",
        "nomsan",
        "notsan",
    ],
    deps = [
        "//tcmalloc:malloc_extension",
        "//tcmalloc/internal:logging",
        "@com_github_google_benchmark//:benchmark",
        "@com_google_absl//absl/base",
        "@com_google_absl//absl/base:core_headers",
        "@com_google_absl//absl/container:flat_hash_map",
        "@com_google_googletest//:gtest_main",
    ],
)

cc_test(
    name = "memory_errors_test",
    srcs = ["memory_errors_test.cc"],
    copts = TCMALLOC_DEFAULT_COPTS,
    malloc = "//tcmalloc",
    tags = [
        "noasan",
        "nomsan",
        "notsan",
    ],
    deps = [
        "//tcmalloc:common",
        "//tcmalloc:malloc_extension",
        "//tcmalloc/internal:bits",
        "//tcmalloc/internal:declarations",
        "@com_github_google_benchmark//:benchmark",
        "@com_google_absl//absl/base:core_headers",
        "@com_google_absl//absl/memory",
        "@com_google_googletest//:gtest_main",
    ],
)

cc_test(
    name = "get_stats_test",
    srcs = ["get_stats_test.cc"],
    copts = TCMALLOC_DEFAULT_COPTS,
    linkstatic = 1,
    malloc = "//tcmalloc",
    tags = [
        "noasan",
        "nomsan",
        "notsan",
    ],
    deps = [
        ":testutil",
        "//tcmalloc:common",
        "//tcmalloc:malloc_extension",
        "@com_github_google_benchmark//:benchmark",
        "@com_google_googletest//:gtest_main",
    ],
)

cc_test(
    name = "releasing_test",
    srcs = ["releasing_test.cc"],
    copts = TCMALLOC_DEFAULT_COPTS,
    malloc = "//tcmalloc",
    tags = [
        "noasan",
        "nomsan",
        "notsan",
    ],
    deps = [
        "//tcmalloc:common",
        "//tcmalloc:malloc_extension",
        "//tcmalloc:want_hpaa",
        "//tcmalloc/internal:logging",
        "//tcmalloc/internal:memory_stats",
        "@com_github_google_benchmark//:benchmark",
        "@com_google_absl//absl/random",
        "@com_google_absl//absl/strings",
        "@com_google_absl//absl/types:optional",
        "@com_google_googletest//:gtest",
    ],
)

create_tcmalloc_testsuite(
    name = "default_parameters_test",
    srcs = ["default_parameters_test.cc"],
    copts = TCMALLOC_DEFAULT_COPTS,
    deps = [
        "//tcmalloc:malloc_extension",
        "@com_github_google_benchmark//:benchmark",
        "@com_google_absl//absl/strings:str_format",
    ],
)

cc_binary(
    name = "hello_main",
    testonly = 1,
    srcs = ["hello_main.cc"],
    malloc = "//tcmalloc",
    deps = [
        "//tcmalloc:malloc_extension",
        "@com_google_absl//absl/strings",
        "@com_google_absl//absl/strings:str_format",
        "@com_google_absl//absl/types:optional",
    ],
)
