load("@bazel_tools//tools/build_defs/repo:http.bzl", "http_archive")

http_archive(
    name = "build_bazel_rules_apple",
    sha256 = "36072d4f3614d309d6a703da0dfe48684ec4c65a89611aeb9590b45af7a3e592",
    url = "https://github.com/bazelbuild/rules_apple/releases/download/1.0.1/rules_apple.1.0.1.tar.gz",
)

load(
    "@build_bazel_rules_apple//apple:repositories.bzl",
    "apple_rules_dependencies",
)

apple_rules_dependencies()

http_archive(
    name = "build_bazel_rules_swift",
    sha256 = "12057b7aa904467284eee640de5e33853e51d8e31aae50b3fb25d2823d51c6b8",
    url = "https://github.com/bazelbuild/rules_swift/releases/download/1.0.0/rules_swift.1.0.0.tar.gz",
)

load(
    "@build_bazel_rules_swift//swift:repositories.bzl",
    "swift_rules_dependencies",
)

swift_rules_dependencies()

load(
    "@build_bazel_rules_swift//swift:extras.bzl",
    "swift_rules_extra_dependencies",
)

swift_rules_extra_dependencies()

load(
    "@build_bazel_apple_support//lib:repositories.bzl",
    "apple_support_dependencies",
)

apple_support_dependencies()

http_archive(
    name = "GoogleUtilities",
    sha256 = "19e3e6363cbb667946391f4a6c81b433978fd936d365ecc471a802d45af6e20d",
    strip_prefix = "GoogleUtilities-2eadf6ab0b040c932b35de6919ce3c345c3f9988",
    url = "https://github.com/amonshiz/GoogleUtilities/archive/2eadf6ab0b040c932b35de6919ce3c345c3f9988.tar.gz",
)

http_archive(
    name = "Promises",
    sha256 = "c7d12165d51cf39b82a173141af49aa76cb72a783e64bfd74e38a2a936ff6b12",
    strip_prefix = "promises-2.1.1",
    url = "https://github.com/amonshiz/promises/archive/refs/tags/2.1.1.tar.gz",
)

http_archive(
    name = "GoogleDataTransport",
    sha256 = "401af1a2dd561836dda9704b10841bc38870f73fad407f1dad50f2a9427982ac",
    strip_prefix = "GoogleDataTransport-3f19738496152e2c21ee45ab70569593806edbe0",
    url = "https://github.com/amonshiz/GoogleDataTransport/archive/3f19738496152e2c21ee45ab70569593806edbe0.tar.gz",
)

http_archive(
    name = "nanopb",
    sha256 = "526dbd98c3c67a74c8158512112ddea9ed332b47e11508849d02ec9c56ba6633",
    strip_prefix = "nanopb-5cc2fbc6384da26e880c35f11662f75f944eb0c2",
    url = "https://github.com/amonshiz/nanopb/archive/5cc2fbc6384da26e880c35f11662f75f944eb0c2.tar.gz",
)

http_archive(
    name = "GTMSessionFetcher",
    sha256 = "e5efbde63c6f0340a4cf4fa27898ad3f89af5342fae57246d968cc1af74b7a71",
    strip_prefix = "gtm-session-fetcher-370f7d15650518acf571f6551583a74aa1fc2273",
    url = "https://github.com/amonshiz/gtm-session-fetcher/archive/370f7d15650518acf571f6551583a74aa1fc2273.tar.gz",
)

http_archive(
    name = "abseil",
    sha256 = "664e6e1613d71b49d7ae4af1e0c41c33c907fa6531b892ed3bca5080535fe1bc",
    strip_prefix = "abseil-cpp-SwiftPM-583de9bd60f66b40e78d08599cc92036c2e7e4e1",
    url = "https://github.com/firebase/abseil-cpp-SwiftPM/archive/583de9bd60f66b40e78d08599cc92036c2e7e4e1.tar.gz",
)

http_archive(
    name = "com_github_grpc_grpc",
    sha256 = "c24ff8983f33b791acfc2078740acaadfc19ab3c40bcbbfaabb059234811241e",
    strip_prefix = "grpc-ios-8440b914756e0d26d4f4d054a1c1581daedfc5b6/native_src",
    url = "https://github.com/grpc/grpc-ios/archive/8440b914756e0d26d4f4d054a1c1581daedfc5b6.tar.gz",
)

load("@com_github_grpc_grpc//bazel:grpc_deps.bzl", "grpc_deps")

grpc_deps()

load("@com_github_grpc_grpc//bazel:grpc_extra_deps.bzl", "grpc_extra_deps")

grpc_extra_deps()

local_repository(
    name = "leveldb",
    path = "../leveldb",
)
