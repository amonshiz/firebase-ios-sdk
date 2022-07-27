load("@build_bazel_rules_swift//swift:swift.bzl", "swift_library")

package(default_visibility = ["//visibility:public"])

NANOPB_DEFINES = [
    "PB_FIELD_32BIT=1",
    "PB_NO_PACKED_STRUCTS=1",
    "PB_ENABLE_MALLOC=1",
]

objc_library(
    name = "Firebase",
    hdrs = glob([
        "CoreOnly/Sources/**/*.h",
    ]),
    module_map = "CoreOnly/Sources/module.modulemap",
)

swift_library(
    name = "FirebaseCoreInternal",
    srcs = glob([
        "FirebaseCore/Internal/Sources/**/*.swift",
    ]),
    defines = [
        # Hack to address the SPM specific import
        # "SWIFT_PACKAGE",
        "FIREBASE_BUILD_CMAKE=1",
        "BAZEL_BUILD",
    ],
    module_name = "FirebaseCoreInternal",
    deps = [
        "@GoogleUtilities//:NSData",
    ],
)

objc_library(
    name = "FirebaseCoreDiagnostics",
    srcs = glob([
        "Firebase/CoreDiagnostics/FIRCDLibrary/**/*.h",
        "Firebase/CoreDiagnostics/FIRCDLibrary/**/*.m",
        "Firebase/CoreDiagnostics/FIRCDLibrary/**/*.c",
        "Interop/CoreDiagnostics/Public/*.h",
    ]),
    hdrs = glob([
        "Firebase/CoreDiagnostics/FIRCDLibrary/Public/**/*.h",
    ]),
    defines = NANOPB_DEFINES,
    includes = [
        "Firebase/CoreDiagnostics/FIRCDLibrary/Public",
    ],
    sdk_dylibs = [
        "Foundation",
    ],
    deps = [
        "@GoogleDataTransport",
        "@GoogleUtilities",
        "@nanopb",
    ],
)

objc_library(
    name = "FirebaseCore",
    srcs = glob([
        "FirebaseCore/Sources/**/*.h",
        "FirebaseCore/Sources/**/*.m",
        "FirebaseCore/Extension/*.h",
        "Interop/CoreDiagnostics/Public/*.h",
    ]),
    hdrs = glob([
        "FirebaseCore/Sources/Public/FirebaseCore/*.h",
    ]),
    copts = [
        "-std=c99",
        "-fno-autolink",
    ],
    defines = [
        "FIREBASE_BUILD_CMAKE=1",
        'Firebase_VERSION="9.4.0"',
    ],
    enable_modules = True,
    includes = [
        "FirebaseCore/Sources/Public",
    ],
    module_name = "FirebaseCore",
    sdk_frameworks = [
        "Foundation",
        "AppKit",
    ],
    deps = [
        ":Firebase",
        ":FirebaseCoreDiagnostics",
        ":FirebaseCoreInternal",
        "@GoogleUtilities",
    ],
)

objc_library(
    name = "FirebaseAppCheckInterop",
    srcs = [
        "FirebaseAppCheck/Interop/dummy.m",
    ],
    hdrs = glob([
        "FirebaseAppCheck/Interop/*.h",
    ]),
)

objc_library(
    name = "FirebaseAuthInterop",
    srcs = [
        "FirebaseAuth/Interop/dummy.m",
    ],
    hdrs = [
        "FirebaseAuth/Interop/FIRAuthInterop.h",
    ],
)

objc_library(
    name = "FirebaseAuth",
    srcs = glob([
        "FirebaseAuth/Sources/**/*.h",
        "FirebaseAuth/Sources/*.m",
        "FirebaseCore/Extension/*.h",
        "FirebaseAuth/Interop/*.h",
    ]),
    hdrs = glob([
        "FirebaseAuth/Sources/Public/FirebaseAuth/**/*.h",
    ]),
    copts = [
        "-std=c99",
    ],
    defines = [
        "BAZEL_BUILD",
    ],
    enable_modules = True,
    includes = [
        "FirebaseAuth/Sources/Public",
    ],
    sdk_dylibs = [
        "Security",
    ],
    deps = [
        ":FirebaseCore",
        "@GTMSessionFetcher//:Core",
        "@GoogleUtilities",
    ],
)

cc_library(
    name = "FirestoreProtosNanopb",
    srcs = glob([
        "Firestore/Protos/nanopb/**/*.cc",
        "Firestore/Protos/nanopb/**/*.h",
        "Firestore/core/src/**/*.cc",
        "Firestore/core/src/**/*.h",
        "Firestore/core/include/**/*.h",
    ]),
    hdrs = glob([
        "Firestore/Protos/nanopb/**/*.h",
    ]) + [
        "Firestore/third_party/nlohmann_json/json.hpp",
    ],
    copts = [
        "-std=gnu++14",
    ],
    defines = NANOPB_DEFINES + [
        "BAZEL_BUILD",
        'FIRFirestore_VERSION="9.3.0"',
    ],
    includes = [
        "Firestore/Protos/nanopb",
    ],
    deps = [
        "@abseil//absl/strings",
        "@abseil//absl/types:any",
        "@abseil//absl/types:optional",
        "@com_github_grpc_grpc//:grpc++",
        "@leveldb",
        "@nanopb",
    ],
)

objc_library(
    name = "FirestoreCoreUtil",
    srcs = glob(
        include = [
            "Firestore/core/src/util/*.cc",
            "Firestore/core/src/util/*.h",
            "Firestore/core/src/util/*.mm",
            "Firestore/core/src/util/executor_libdispatch.*",
        ],
        exclude = [
            "Firestore/core/src/util/*_win.cc",
            "Firestore/core/src/util/executor_*",
            "Firestore/core/src/util/log_*",
            "Firestore/core/src/util/secure_random_*.cc",
        ],
    ) + [
        "Firestore/core/src/util/log_stdio.cc",
        "Firestore/core/src/util/secure_random_arc4random.cc",
    ],
    hdrs = glob([
        "Firestore/core/src/util/*.h",
        "Firestore/core/include/firebase/firestore/*.h",
        "Firestore/core/src/objc/*.h",
    ]),
    sdk_dylibs = [
        "CoreFoundation",
        "Foundation",
    ],
    deps = [
        ":FirebaseCore",
        "@abseil//absl/algorithm:container",
        "@abseil//absl/base",
        "@abseil//absl/strings",
    ],
)

NANOPB_SRCS_GLOB_PATTERNS = [
    "Firestore/core/src/nanopb/byte_string.*",
    "Firestore/core/src/nanopb/nanopb_util.*",
    "Firestore/core/src/nanopb/pretty_printing.*",
]

cc_library(
    name = "FirestoreCoreNanopb",
    srcs = glob(NANOPB_SRCS_GLOB_PATTERNS),
    defines = NANOPB_DEFINES,
    deps = [
        ":FirestoreCoreUtil",
        "@abseil//absl/strings",
        "@abseil//absl/types:optional",
        "@nanopb",
    ],
)

objc_library(
    name = "FirestoreCore",
    srcs = glob(
        include = [
            "Firestore/core/include/firebase/firestore/*.h",
            "Firestore/core/src/*.cc",
            "Firestore/core/src/*.h",
            "Firestore/core/src/api/*.cc",
            "Firestore/core/src/api/*.h",
            "Firestore/core/src/bundle/*.cc",
            "Firestore/core/src/bundle/*.h",
            "Firestore/core/src/core/*.cc",
            "Firestore/core/src/core/*.h",
            "Firestore/core/src/credentials/*.cc",
            "Firestore/core/src/credentials/*.h",
            "Firestore/core/src/immutable/*.cc",
            "Firestore/core/src/immutable/*.h",
            "Firestore/core/src/index/*.cc",
            "Firestore/core/src/index/*.h",
            "Firestore/core/src/local/*.cc",
            "Firestore/core/src/local/*.h",
            "Firestore/core/src/model/*.cc",
            "Firestore/core/src/model/*.h",
            "Firestore/core/src/model/mutation/*.cc",
            "Firestore/core/src/model/mutation/*.h",
            "Firestore/core/src/nanopb/*.cc",
            "Firestore/core/src/nanopb/*.h",
            "Firestore/core/src/objc/*.h",
            "Firestore/core/src/remote/*.cc",
            "Firestore/core/src/remote/*.h",
            "Firestore/core/src/credentials/firebase_app_check_credentials_provider_apple.*",
            "Firestore/core/src/credentials/firebase_auth_credentials_provider_apple.*",
            "Firestore/core/src/remote/connectivity_monitor_apple.mm",
            "Firestore/core/src/remote/firebase_metadata_provider_apple.mm",
        ],
        exclude = NANOPB_SRCS_GLOB_PATTERNS,
    ),
    hdrs = glob([
        "Firestore/core/include/**/*.h",
    ]),
    copts = [
        "-std=gnu++14",
    ],
    defines = NANOPB_DEFINES,
    includes = [
        "Firestore/core/include",
    ],
    sdk_dylibs = [
        "Foundation",
        "SystemConfiguration",
    ],
    deps = [
        ":FirebaseAppCheckInterop",
        ":FirebaseAuthInterop",
        ":FirebaseCore",
        ":FirestoreCoreNanopb",
        ":FirestoreCoreUtil",
        ":FirestoreProtosNanopb",
        "@leveldb",
        "@nanopb",
    ],
)

objc_library(
    name = "FIRTimestamp",
    srcs = [
        "Firestore/Source/API/FIRTimestamp.m",
        "Firestore/Source/API/FIRTimestamp+Internal.h",
    ],
    hdrs = [
        "Firestore/Source/Public/FirebaseFirestore/FIRTimestamp.h",
    ],
    copts = [
        "-std=c99",
    ],
    includes = [
        "Firestore/Source/Public",
        "Firestore/Source/Public/FirebaseFirestore",
    ],
)

objc_library(
    name = "FirebaseFirestore",
    srcs = glob([
        "Firestore/Source/API/*.h",
        "Firestore/Source/API/*.m",
        "Firestore/Source/API/*.mm",
    ]),
    hdrs = glob([
        "Firestore/Source/Public/FirebaseFirestore/*.h",
    ]),
    includes = [
        "Firestore/Source/Public",
        "Firestore/Source/Public/FirebaseFirestore",
    ],
    deps = [
        ":FirebaseAuthInterop",
        ":FirebaseCore",
        ":FirestoreCore",
        "@abseil//absl/strings",
    ],
)

swift_library(
    name = "FirebaseFirestoreSwift",
    srcs = glob([
        "Firestore/Swift/Source/**/*.swift",
        "Firestore/third_party/FirestoreEncoder/*.swift",
    ]),
    module_name = "FirebaseFirestoreSwift",
    deps = [
        ":FirebaseFirestore",
        "@GoogleUtilities",
    ],
)

objc_library(
    name = "FirebaseStorageInternal",
    srcs = glob([
        "FirebaseStorageInternal/Sources/*.h",
        "FirebaseStorageInternal/Sources/*.m",
    ]),
    hdrs = glob([
        "FirebaseStorageInternal/Sources/Public/FirebaseStorageInternal/*.h",
    ]),
    includes = [
        "FirebaseStorageInternal/Sources/Public",
    ],
    deps = [
        ":FirebaseAppCheckInterop",
        ":FirebaseAuthInterop",
        ":FirebaseCore",
        "@GTMSessionFetcher",
    ],
)

objc_library(
    name = "FirebaseCoreExtension",
    srcs = [
        "FirebaseCore/Extension/dummy.m",
    ],
    hdrs = glob([
        "FirebaseCore/Extension/*.h",
    ]),
    includes = [
        "FirebaseCore/Extension",
    ],
    module_name = "FirebaseCoreExtension",
)

swift_library(
    name = "FirebaseStorage",
    srcs = glob([
        "FirebaseStorage/Sources/**/*.swift",
    ]),
    module_name = "FirebaseStorage",
    deps = [
        ":FirebaseCoreExtension",
        ":FirebaseStorageInternal",
    ],
)
