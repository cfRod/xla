load("//xla/tsl:tsl.bzl", "internal_visibility")
load("//xla/tsl/platform:rules_cc.bzl", "cc_library")

package(
    # copybara:uncomment default_applicable_licenses = ["//tensorflow:license"],
    default_visibility = internal_visibility([":friends"]),
    licenses = ["notice"],
)

package_group(
    name = "friends",
    includes = [
        "//xla:friends",
    ],
)

cc_library(
    name = "cpu_clique_key",
    srcs = ["cpu_clique_key.cc"],
    hdrs = ["cpu_clique_key.h"],
    deps = [
        "//xla/core/collectives:clique_key",
        "//xla/service:global_device_id",
        "@com_google_absl//absl/algorithm:container",
        "@com_google_absl//absl/hash",
        "@com_google_absl//absl/strings",
        "@com_google_absl//absl/strings:str_format",
        "@tsl//tsl/platform:casts",
    ],
)

cc_library(
    name = "cpu_clique",
    srcs = ["cpu_clique.cc"],
    hdrs = ["cpu_clique.h"],
    deps = [
        ":cpu_clique_key",
        "//xla/core/collectives:clique",
        "//xla/core/collectives:communicator",
        "//xla/core/collectives:rank_id",
        "//xla/tsl/platform:logging",
        "@com_google_absl//absl/status",
        "@com_google_absl//absl/strings",
        "@com_google_absl//absl/strings:str_format",
    ],
)

cc_library(
    name = "cpu_cliques",
    srcs = ["cpu_cliques.cc"],
    hdrs = ["cpu_cliques.h"],
    deps = [
        ":cpu_clique",
        ":cpu_clique_key",
        ":cpu_collectives",
        "//xla:util",
        "//xla/core/collectives:clique",
        "//xla/core/collectives:communicator",
        "//xla/core/collectives:rank_id",
        "//xla/tsl/platform:errors",
        "//xla/tsl/platform:logging",
        "//xla/tsl/platform:statusor",
        "@com_google_absl//absl/base:core_headers",
        "@com_google_absl//absl/container:node_hash_map",
        "@com_google_absl//absl/status:statusor",
        "@com_google_absl//absl/synchronization",
    ],
)

cc_library(
    name = "cpu_collectives",
    srcs = ["cpu_collectives.cc"],
    hdrs = ["cpu_collectives.h"],
    deps = [
        "//xla:shape_util",
        "//xla:util",
        "//xla:xla_data_proto_cc",
        "//xla/core/collectives",
        "//xla/core/collectives:clique_id",
        "//xla/core/collectives:collectives_registry",
        "//xla/core/collectives:communicator",
        "//xla/core/collectives:rank_id",
        "//xla/service:collective_ops_utils",
        "@com_google_absl//absl/log",
        "@com_google_absl//absl/log:check",
        "@com_google_absl//absl/status",
        "@com_google_absl//absl/status:statusor",
        "@com_google_absl//absl/time",
        "@com_google_absl//absl/types:span",
        "@tsl//tsl/platform:casts",
    ],
)

# TODO(b/380457503): Restrict visibility to private.
cc_library(
    name = "gloo_communicator",
    srcs = ["gloo_communicator.cc"],
    hdrs = ["gloo_communicator.h"],
    copts = [
        "-fexceptions",
        "-fno-strict-aliasing",
    ],
    features = ["-use_header_modules"],
    deps = [
        ":cpu_collectives",
        "//xla:shape_util",
        "//xla:status_macros",
        "//xla:types",
        "//xla:util",
        "//xla:xla_data_proto_cc",
        "//xla/core/collectives:communicator",
        "//xla/core/collectives:rank_id",
        "//xla/service:collective_ops_utils",
        "//xla/service:global_device_id",
        "//xla/service/cpu:collectives_interface",
        "//xla/stream_executor:device_memory",
        "//xla/tsl/platform:errors",
        "//xla/tsl/platform:statusor",
        "@com_google_absl//absl/base:core_headers",
        "@com_google_absl//absl/container:flat_hash_map",
        "@com_google_absl//absl/log",
        "@com_google_absl//absl/status",
        "@com_google_absl//absl/status:statusor",
        "@com_google_absl//absl/strings",
        "@com_google_absl//absl/synchronization",
        "@com_google_absl//absl/time",
        "@com_google_absl//absl/types:span",
        "@gloo",
        "@tsl//tsl/platform:errors",
        "@tsl//tsl/platform:logging",
    ],
)

# TODO(b/380457503): Restrict visibility to private.
cc_library(
    name = "in_process_communicator",
    srcs = ["in_process_communicator.cc"],
    hdrs = ["in_process_communicator.h"],
    deps = [
        ":cpu_collectives",
        "//xla:refcounting_hash_map",
        "//xla:shape_util",
        "//xla:status_macros",
        "//xla:util",
        "//xla:xla_data_proto_cc",
        "//xla/core/collectives:communicator",
        "//xla/core/collectives:rank_id",
        "//xla/service:collective_ops_utils",
        "//xla/service:global_device_id",
        "//xla/stream_executor:device_memory",
        "//xla/tsl/platform:statusor",
        "@com_google_absl//absl/log",
        "@com_google_absl//absl/status",
        "@com_google_absl//absl/status:statusor",
        "@com_google_absl//absl/strings",
        "@com_google_absl//absl/strings:str_format",
        "@com_google_absl//absl/time",
        "@com_google_absl//absl/types:span",
        "@tsl//tsl/platform:errors",
    ],
)

# TODO(b/380457503): Restrict visibility to private.
cc_library(
    name = "mpi_communicator",
    srcs = ["mpi_communicator.cc"],
    hdrs = ["mpi_communicator.h"],
    compatible_with = [],
    copts = [
        "-fexceptions",
        "-fno-strict-aliasing",
        # copybara:uncomment_begin(google-only)
        # "-Ithird_party/openmpi/ompi/include",
        # copybara:uncomment_end
    ],
    features = ["-use_header_modules"],
    deps = [
        "//xla:shape_util",
        "//xla:status_macros",
        "//xla:types",
        "//xla:util",
        "//xla:xla_data_proto_cc",
        "//xla/core/collectives:communicator",
        "//xla/core/collectives:rank_id",
        "//xla/service:collective_ops_utils",
        "//xla/service:global_device_id",
        "//xla/service/cpu:collectives_interface",
        "//xla/stream_executor:device_memory",
        "//xla/tsl/platform:errors",
        "//xla/tsl/platform:logging",
        "//xla/tsl/platform:statusor",
        "@com_google_absl//absl/base:core_headers",
        "@com_google_absl//absl/container:flat_hash_map",
        "@com_google_absl//absl/status",
        "@com_google_absl//absl/status:statusor",
        "@com_google_absl//absl/strings",
        "@com_google_absl//absl/synchronization",
        "@com_google_absl//absl/time",
        "@com_google_absl//absl/types:span",
        "@mpitrampoline",
        "@tsl//tsl/platform:errors",
        "@tsl//tsl/platform:logging",
        "@tsl//tsl/platform:statusor",
    ],
)
