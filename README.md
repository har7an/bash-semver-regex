# Bash-compatible SemVer Regex

A regular expression for use with bash that captures the fragments of the
[Semantic Versioning Specification][1]. This repo also contains a short bash
script that runs the regex against a set of example version numbers to check
its validity.

This uses bash built-in regex handling, which uses POSIX regex. Think of it as
an extension to the [suggested regex FAQ entry][2] for use in bash (which
doesn't have a concept of non-capturing groups).


## The Regex

Terse:

```bash
^(0|[1-9][0-9]*)\.(0|[1-9][0-9]*)\.(0|[1-9][0-9]*)(-((0|[1-9][0-9]*|[0-9]*[a-zA-Z-][0-9a-zA-Z-]*)(\.(0|[1-9][0-9]*|[0-9]*[a-zA-Z-][0-9a-zA-Z-]*))*))?(\+([0-9a-zA-Z-]+(\.[0-9a-zA-Z-]+)*))?$
```

Human-Readable:

```bash
# Regex for a semver digit
D='0|[1-9][0-9]*'
# Regex for a semver pre-release word
PW='[0-9]*[a-zA-Z-][0-9a-zA-Z-]*'
# Regex for a semver build-metadata word
MW='[0-9a-zA-Z-]+'

if [[ "$INPUT" =~ ^($D)\.($D)\.($D)(-(($D|$PW)(\.($D|$PW))*))?(\+($MW(\.$MW)*))?$ ]]; then
    MAJOR="${BASH_REMATCH[1]}"
    MINOR="${BASH_REMATCH[2]:-""}"
    PATCH="${BASH_REMATCH[3]:-""}"
    PRE_RELEASE="${BASH_REMATCH[5]:-""}"
    BUILD_METADATA="${BASH_REMATCH[10]:-""}"
fi
```


## The Capture Groups

The individual parts are captured as follows:

- Capture group 1: Major
- Capture group 2: Minor
- Capture group 3: Patch
- Capture group 5: Prerelease
- Capture group 10: Build metadata


## Original Source

This repo came to life following a [discussion in an issue][3] in the semver
repo on GitHub. See [here][3].


[1]: https://semver.org/
[2]: https://semver.org/#is-there-a-suggested-regular-expression-regex-to-check-a-semver-string
[3]: https://github.com/semver/semver/issues/981
