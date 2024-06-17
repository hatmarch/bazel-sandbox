workspace(name = "rules_python_multi_python_versions")

load("@bazel_tools//tools/build_defs/repo:http.bzl", "http_archive")


# Update the SHA and VERSION to the lastest version available here:
# https://github.com/bazelbuild/rules_python/releases.

SHA="5bcfa3852444d084b1d3262714cec151b797648d4d444ea9895c7c7ed79cd715"

VERSION="0.33.1"

http_archive(
    name = "rules_python",
    sha256 = SHA,
    strip_prefix = "rules_python-{}".format(VERSION),
    url = "https://github.com/bazelbuild/rules_python/releases/download/{}/rules_python-{}.tar.gz".format(VERSION,VERSION),
)

load("@rules_python//python:repositories.bzl", "py_repositories", "python_register_multi_toolchains")

py_repositories()

load("@rules_python//python/pip_install:repositories.bzl", "pip_install_dependencies")

pip_install_dependencies()

default_python_version = "3.9"

python_register_multi_toolchains(
    name = "python",
    default_version = default_python_version,
    python_versions = [
        "3.8",
        "3.9",
        "3.10",
        "3.11",
    ],
    register_coverage_tool = True,
)

load("@python//:pip.bzl", "multi_pip_parse")

multi_pip_parse(
    name = "pypi",
    default_version = default_python_version,
    python_interpreter_target = {
        "3.10": "@python_3_10_host//:python",
        "3.11": "@python_3_11_host//:python",
        "3.8": "@python_3_8_host//:python",
        "3.9": "@python_3_9_host//:python",
    },
    requirements_lock = {
        "3.10": "//requirements:requirements_lock_3_10.txt",
        "3.11": "//requirements:requirements_lock_3_11.txt",
        "3.8": "//requirements:requirements_lock_3_8.txt",
        "3.9": "//requirements:requirements_lock_3_9.txt",
    },
)

load("@pypi//:requirements.bzl", "install_deps")

install_deps()
