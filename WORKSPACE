workspace(name = "pomgen")

load("@bazel_tools//tools/build_defs/repo:http.bzl", "http_archive")

RULES_JVM_EXTERNAL_TAG = "4.1"
RULES_JVM_EXTERNAL_SHA = "f36441aa876c4f6427bfb2d1f2d723b48e9d930b62662bf723ddfb8fc80f0140"

http_archive(
    name = "rules_jvm_external",
    strip_prefix = "rules_jvm_external-%s" % RULES_JVM_EXTERNAL_TAG,
    sha256 = RULES_JVM_EXTERNAL_SHA,
    url = "https://github.com/bazelbuild/rules_jvm_external/archive/%s.zip" % RULES_JVM_EXTERNAL_TAG,
)

load("@rules_jvm_external//:defs.bzl", "maven_install")
load("@rules_jvm_external//:specs.bzl", "maven")


load("@rules_jvm_external//:repositories.bzl", "rules_jvm_external_deps")
rules_jvm_external_deps()
load("@rules_jvm_external//:setup.bzl", "rules_jvm_external_setup")
rules_jvm_external_setup()

maven_install(
    name = "maven",
    artifacts = [
        maven.artifact(group = "com.google.guava", artifact = "guava", version = "23.0", exclusions = ["*:*"]),
        maven.artifact(group = "org.apache.commons", artifact = "commons-lang3", version = "3.9", exclusions = ["*:*"]),
        maven.artifact(group = "org.apache.commons", artifact = "commons-math3", version = "3.6.1", exclusions = ["*:*"]),
        maven.artifact(group = "org.antlr", artifact = "stringtemplate", version = "3.2.1",),
        maven.artifact(group = "org.antlr", artifact = "ST4", version = "4.0.7",exclusions = ["antlr:antlr"]),
    ],
    repositories = [
        "https://repo1.maven.org/maven2",
    ],
    version_conflict_policy = "pinned",
    strict_visibility = True,
    generate_compat_repositories = False,
    maven_install_json = "//:maven_install.json",  # regenerate: bazel run @unpinned_maven//:pin
    resolve_timeout = 1800,
)

load("@maven//:defs.bzl", "pinned_maven_install")
pinned_maven_install()

# this rule is here to test pomgen with multipe maven_install rules
maven_install(
    name = "antlr",
    artifacts = [
        maven.artifact(group = "org.antlr", artifact = "ST4", version = "4.0.7",),
    ],
    repositories = [
        "https://repo1.maven.org/maven2",
    ],
    version_conflict_policy = "pinned",
    strict_visibility = True,
    generate_compat_repositories = False,
    maven_install_json = "//:antlr_install.json",  # regenerate: bazel run @unpinned_antlr//:pin
    resolve_timeout = 1800,
)

load("@antlr//:defs.bzl", antlr_pinned = "pinned_maven_install")
antlr_pinned()
