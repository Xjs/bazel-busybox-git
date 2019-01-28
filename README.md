# Bazel + Busybox + `git_repository`

Built with `bash` as `BAZEL_SH`, this repository will work as intended:

```$ bazel build main
(18:49:43) INFO: Invocation ID: c1f69a03-d75e-4915-9425-a57da9e3e6a7
(18:49:43) INFO: Current date is 2019-01-28
(18:49:50) INFO: Rule 'com_github_Xjs_dummy_dependency' modified arguments {"shallow_since": "1548697538 +0100"}
(18:49:51) INFO: Analysed target //:main (9 packages loaded, 69 targets configured).
(18:49:51) INFO: Found 1 target...
Target //:main up-to-date:
  C:/tmp/_bazel_jannis.schnitzer/fji6e52k/execroot/com_github_Xjs_bazel_busybox_git/bazel-out/x64_windows-fastbuild/bin/main.exe
(18:49:51) INFO: Elapsed time: 9,951s, Critical Path: 0,32s
(18:49:51) INFO: 4 processes: 4 local.
(18:49:51) INFO: Build completed successfully, 9 total actions
```

However, when trying to use [busybox-w32](https://github.com/rmyorston/busybox-w32) as a more lightweight `BAZEL_SH`, it will (even with tip, which includes a fix for https://github.com/rmyorston/busybox-w32/issues/138) fail as follows:

```$ bazel build main
(18:51:30) INFO: Invocation ID: ee11d830-2f72-4e2b-90fe-b6e49a02bcd9
(18:51:30) INFO: Current date is 2019-01-28
(18:51:35) ERROR: C:/users/jannis.schnitzer/src/github.com/xjs/bazel-busybox-git/BUILD.bazel:1:1: no such package '@com_github_Xjs_dummy_dependency//': Traceback (most recent call last):
        File "C:/tmp/_bazel_jannis.schnitzer/fji6e52k/external/bazel_tools/tools/build_defs/repo/git.bzl", line 170
                _clone_or_update(ctx)
        File "C:/tmp/_bazel_jannis.schnitzer/fji6e52k/external/bazel_tools/tools/build_defs/repo/git.bzl", line 73, in _clone_or_update
                fail(("error cloning %s:\n%s" % (ctx....)))
error cloning com_github_Xjs_dummy_dependency:
+ cd C:/tmp/_bazel_jannis.schnitzer/fji6e52k/external
+ rm -rf C:/tmp/_bazel_jannis.schnitzer/fji6e52k/external/com_github_Xjs_dummy_dependency C:/tmp/_bazel_jannis.schnitzer/fji6e52k/external/com_github_Xjs_dummy_dependency
rm: can't remove 'C:/tmp/_bazel_jannis.schnitzer/fji6e52k/external/com_github_Xjs_dummy_dependency': Permission denied
rm: can't remove 'C:/tmp/_bazel_jannis.schnitzer/fji6e52k/external/com_github_Xjs_dummy_dependency': Permission denied
 and referenced by '//:main'
(18:51:35) WARNING: errors encountered while analyzing target '//:main': it will not be built
(18:51:35) INFO: Analysed target //:main (8 packages loaded, 66 targets configured).
(18:51:35) INFO: Found 0 targets...
(18:51:35) ERROR: command succeeded, but there were loading phase errors
(18:51:35) INFO: Elapsed time: 7,360s, Critical Path: 0,01s
(18:51:35) INFO: 0 processes.
(18:51:35) FAILED: Build did NOT complete successfully
    Fetching @com_github_Xjs_dummy_dependency; Cloning 23e3778ac46bedd3057c0b25942d59c5f5ea4884 of https://github.com/Xjs/dummy-dependency
```
