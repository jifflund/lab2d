load("//:build_defs.bzl", "pybind_extension", "pytype_strict_library")
load("//:wheel_defs.bzl", "py_wheel")
load("@python_system//:defs.bzl", "PY_TAGS")

licenses(["notice"])

pybind_extension(
    name = "dmlab2d_pybind",
    srcs = ["dmlab2d_pybind.cc"],
    copts = ["-fexceptions"],
    deps = [
        "//dmlab2d/lib:dmlab2d",
        "//dmlab2d/lib/support:logging",
        "//third_party/rl_api:env_c_api",
        "@com_google_absl//absl/container:flat_hash_map",
        "@com_google_absl//absl/strings",
        "@com_google_absl//absl/types:span",
    ],
)

pytype_strict_library(
    name = "dmlab2d",
    srcs = ["__init__.py"],
    visibility = ["//visibility:public"],
    deps = [
        ":dmlab2d_pybind",
        ":expect_numpy_installed",
        "@dm_env_archive//:dm_env",
        "@six_archive//:six",
    ],
)

py_test(
    name = "dmlab2d_test",
    srcs = ["dmlab2d_test.py"],
    python_version = "PY3",
    srcs_version = "PY3",
    deps = [
        ":dmlab2d",
        ":expect_numpy_installed",
        ":runfiles_helper",
        "@com_google_absl_py//absl/testing:absltest",
        "@dm_env_archive//:test_utils",
    ],
)

pytype_strict_library(
    name = "settings_helper",
    srcs = ["settings_helper.py"],
    visibility = ["//visibility:public"],
)

py_test(
    name = "settings_helper_test",
    srcs = ["settings_helper_test.py"],
    deps = [
        ":settings_helper",
        "@com_google_absl_py//absl/testing:absltest",
    ],
)

pytype_strict_library(
    name = "ui_renderer",
    srcs = ["ui_renderer.py"],
    visibility = ["//visibility:public"],
    deps = [
        ":dmlab2d",
        ":expect_dataclasses_installed",
        ":expect_numpy_installed",
        ":expect_pygame_installed",
        ":expect_typing_installed",
        ":runfiles_helper",
        "@enum34_archive//:enum",
    ],
)

py_binary(
    name = "random_agent",
    srcs = ["random_agent.py"],
    python_version = "PY3",
    srcs_version = "PY3",
    deps = [
        ":dmlab2d",
        ":expect_numpy_installed",
        ":expect_pygame_installed",
        ":runfiles_helper",
    ],
)

pytype_strict_library(
    name = "runfiles_helper",
    srcs = ["runfiles_helper.py"],
    visibility = ["//visibility:public"],
    deps = [":dmlab2d_pybind"],
)

py_library(name = "expect_numpy_installed")

py_library(name = "expect_pygame_installed")

py_library(name = "expect_dataclasses_installed")

py_library(name = "expect_typing_installed")

py_wheel(
    name = "dmlab2d_wheel",
    abi = PY_TAGS.abi,
    distribution = "dmlab2d",
    platform = PY_TAGS.platform,
    python_tag = PY_TAGS.interpreter,
    version = "1.0",
    deps = [
        ":dmlab2d",
        ":runfiles_helper",
        ":settings_helper",
        ":ui_renderer",
    ],
)

# This version of the wheel file does not contain any ABI and platform
# constraints. It is not meant for deployment, but can be used on the
# local machine if it is pip-installed on the same environment that
# was used to build it.
py_wheel(
    name = "dmlab2d_wheel_local",
    distribution = "dmlab2d",
    python_tag = "py3",
    version = "1.0",
    deps = [
        ":dmlab2d",
        ":runfiles_helper",
        ":settings_helper",
        ":ui_renderer",
    ],
)
