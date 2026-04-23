load("@rules_cc//cc:defs.bzl", "cc_library")

package(default_visibility = ["//visibility:public"])

cc_library(
    name = "geom",
    srcs = ["geom/2d/polyline_2d.cpp"],
    hdrs = glob([
        "geom/common/*.h",
        "geom/2d/*.h",
        "geom/geometry_legacy.h",
    ]),
    includes = ["."],
    deps = [
        "@glog//:glog",
    ],
    copts = ["-std=c++17"],
)