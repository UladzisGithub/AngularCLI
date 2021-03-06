# Copyright Google Inc. All Rights Reserved.
#
# Use of this source code is governed by an MIT-style license that can be
# found in the LICENSE file at https://angular.io/license

licenses(["notice"])  # MIT

load("@build_bazel_rules_typescript//:defs.bzl", "ts_library")
load("@build_bazel_rules_nodejs//:defs.bzl", "jasmine_node_test")
load("//tools:ts_json_schema.bzl", "ts_json_schema")

package(default_visibility = ["//visibility:public"])

# Create a list of Tuple("path/file.json", "path_file") to be used as rules
ALL_SCHEMA_TARGETS_PAIR = [
    (x, x.replace("/", "_").replace("-", "_").replace(".json", ""))
    for x in glob(["*/schema.json"])
]

# Create all the targets.
[
    ts_json_schema(
        name = name,
        src = src,
    )
    for (src, name) in ALL_SCHEMA_TARGETS_PAIR
]

ALL_SCHEMA_DEPS = [":" + name for (_, name) in ALL_SCHEMA_TARGETS_PAIR]

ts_library(
    name = "angular",
    srcs = glob(
        include = ["**/*.ts"],
        exclude = [
            "**/*_spec.ts",
            "**/*_spec_large.ts",
            "**/*_benchmark.ts",
            # Also exclude templated files.
            "*/files/**/*.ts",
            "*/other-files/**/*.ts",
            # Exclude test helpers.
            "utility/test/**/*.ts"
        ],
    ),
    deps = ALL_SCHEMA_DEPS + [
        "//packages/angular_devkit/core",
        "//packages/angular_devkit/schematics",
        "//packages/angular_devkit/schematics:tasks",
        "@rxjs",
        "@rxjs//operators",
        # @typings: typescript
        # @typings: node
    ],
)


ts_library(
    name = "angular_test_lib",
    srcs = glob(
        include = [
            "**/*_spec.ts",
            "**/*_spec_large.ts",
            "utility/test/**/*.ts"
        ],
    ),
    data = glob(
        include = [
            "**/*.json",
            "*/files/**",
            "*/other-files/**",
        ]
    ),
    deps = ALL_SCHEMA_DEPS + [
        ":angular",
        "//packages/angular_devkit/core",
        "//packages/angular_devkit/schematics",
        "//packages/angular_devkit/schematics:testing",
        "@rxjs",
        "@rxjs//operators",
        # @typings: jasmine
        # @typings: node
    ],
)

jasmine_node_test(
    name = "angular_test",
    srcs = [":angular_test_lib"],
)
