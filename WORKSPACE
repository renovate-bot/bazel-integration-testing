workspace(name = "build_bazel_integration_testing")

load("@bazel_tools//tools/build_defs/repo:git.bzl", "git_repository")
load("@bazel_tools//tools/build_defs/repo:http.bzl", "http_archive")

#Remote execution infra
# Required configuration for remote build execution
bazel_toolchains_version="31b5dc8c4e9c7fd3f5f4d04c6714f2ce87b126c1"
bazel_toolchains_sha256="07a81ee03f5feae354c9f98c884e8e886914856fb2b6a63cba4619ef10aaaf0b"
http_archive(
         name = "bazel_toolchains",
         urls = [
           "https://mirror.bazel.build/github.com/bazelbuild/bazel-toolchains/archive/%s.tar.gz"%bazel_toolchains_version,
           "https://github.com/bazelbuild/bazel-toolchains/archive/%s.tar.gz"%bazel_toolchains_version
         ],
         strip_prefix = "bazel-toolchains-%s"%bazel_toolchains_version,
         sha256 = bazel_toolchains_sha256,
)

## Sanity checks

git_repository(
    name = "bazel_skylib",
    commit = "ff23a62c57d2912c3073a69c12f42c3d6e58a957",
    remote = "https://github.com/bazelbuild/bazel-skylib",
)

load("@bazel_skylib//:lib.bzl", "versions")

versions.check("0.6.0")

## Linting

load("//private:format.bzl", "format_repositories")

format_repositories()

#### Fetch remote resources

## Python

http_archive(
    name = "com_google_python_gflags",
    build_file_content = """
py_library(
    name = "gflags",
    srcs = [
        "gflags.py",
        "gflags_validators.py",
    ],
    visibility = ["//visibility:public"],
)
""",
    sha256 = "344990e63d49b9b7a829aec37d5981d558fea12879f673ee7d25d2a109eb30ce",
    strip_prefix = "python-gflags-python-gflags-2.0",
    url = "https://github.com/google/python-gflags/archive/python-gflags-2.0.zip",
)

## Java

maven_jar(
    name = "com_google_truth",
    artifact = "com.google.truth:truth:jar:0.31",
)

maven_jar(
    name = "com_spotify_hamcrest_optional",
    artifact = "com.spotify:hamcrest-optional:jar:1.1.1",
)

## golang

load("@bazel_tools//tools/build_defs/repo:http.bzl", "http_archive")

http_archive(
    name = "io_bazel_rules_go",
    sha256 = "b7a62250a3a73277ade0ce306d22f122365b513f5402222403e507f2f997d421",
    urls = ["https://github.com/bazelbuild/rules_go/releases/download/0.16.3/rules_go-0.16.3.tar.gz"],
)

http_archive(
    name = "bazel_gazelle",
    sha256 = "6e875ab4b6bf64a38c352887760f21203ab054676d9c1b274963907e0768740d",
    urls = ["https://github.com/bazelbuild/bazel-gazelle/releases/download/0.15.0/bazel-gazelle-0.15.0.tar.gz"],
)

load("@io_bazel_rules_go//go:def.bzl", "go_rules_dependencies", "go_register_toolchains")

go_rules_dependencies()

go_register_toolchains()

load("@bazel_gazelle//:deps.bzl", "gazelle_dependencies")

gazelle_dependencies()

#### Use remote resources

## java

load("//:bazel_integration_test.bzl", "bazel_java_integration_test_deps")

bazel_java_integration_test_deps()

load("//tools:import.bzl", "bazel_external_dependency_archive")

bazel_external_dependency_archive(
    name = "test_archive",
    srcs = {
        "90a8e1603eeca48e7e879f3afbc9560715322985f39a274f6f6070b43f9d06fe": [
            "http://repo1.maven.org/maven2/junit/junit/4.11/junit-4.11.jar",
            "http://maven.ibiblio.org/maven2/junit/junit/4.11/junit-4.11.jar",
        ],
        "e0de160b129b2414087e01fe845609cd55caec6820cfd4d0c90fabcc7bdb8c1e": [
            "http://repo1.maven.org/maven2/com/beust/jcommander/1.72/jcommander-1.72.jar",
            "http://maven.ibiblio.org/maven2/com/beust/jcommander/1.72/jcommander-1.72.jar",
        ],
    },
)

bazel_external_dependency_archive(
    name = "test_archive2",
    srcs = {
        "91c77044a50c481636c32d916fd89c9118a72195390452c81065080f957de7ff": [
            "http://repo1.maven.org/maven2/javax/inject/javax.inject/1/javax.inject-1.jar",
            "http://maven.ibiblio.org/maven2/javax/inject/javax.inject/1/javax.inject-1.jar",
        ],
    },
)

## Your new language here!
