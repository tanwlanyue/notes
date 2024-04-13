# What is GN?

GN is a meta-build system that generates [NinjaBuild](https://github.com/ninja-build/ninja/blob/master/README.md) files. It's meant to be faster and simpler than GYP. It outputs only Ninja build files.

## Why bother with GN?

1. We believe GN files are more readable and more maintainable than GYP files.
2. GN is fast:

- GN is 20x faster than GYP (as of mid November, building 80% of what GYP builds, in one configuration rather than two, takes 500ms on a z620 running Ubuntu Trusty. GYP takes 20 seconds. We see similar speedups on Mac and Windows).
- GN supports automatically re-running itself as needed by Ninja as part of the build. This eliminates the need to remember to re-run GN when you change a build file.

1. GN gives us better tools for enforcing dependencies (see `gn check` and the `visibility`, `public_deps`, and `data_deps` options for some examples).
2. GN gives us tools for querying the build graph; you can ask “what does X depend on” and “who depends on Y”, for example.

## What's the status?

*as of 8 Feb 2015:*

Chrome and most of the major test suites link on Linux and ChromeOS. There's probably <1000 objects left to build, in a few test suites and a bunch of utillities and helper binaries. We will probably have everything converted in another couple weeks.

Chrome also links on Android and Windows, and bringing up the remaining test suites should be mostly straightforward. There's some work left to enable NaCl on Windows but it should also be straightforward.

Mac and iOS have not progressed much as attention has been focused on Linux and Windows; we still need support for bundles and frameworks before it can get to parity w/ the other platforms.

## When are you going to be done?

*as of 8 Feb 2015:*

We're currently shooting for having the main developer configurations working and usable by the end of March 2015. There will probably be a fair amount of verification of flags, bug fixes, etc over the next couple months, but hopefully we will be flipping configurations from GYP to GN throughout Q2, targeting having everything done by the end of Q2.

## What does “done” mean?

Ideally we're done when all of the GYP files have been deleted from Chromium and no one misses them.

We will be “mostly” done when the following are true:

- All of the bots maintained by the Chrome infra team for Chromium and downstream of Chromium have been switched to GN. (Upstream projects like Skia and V8 can choose to stay on GYP if they like).
- Any developer configurations we care about that don‘t have bots also work (Generally speaking, we’re aiming to not have any of these.
- Configurations we care about should have bots to ensure they don't break). We have verified that all of the tests pass. We have verified that the command lines match in the above configurations as much as possible, and we accept any differences. We have reviewed any binary differences that result in the official builds and accepted them. The GN files are the “source of truth” for the build, and normal chromium developers normally do not need to touch GYP files to keep things working. We have replacements for the hybrid “msvs-ninja” and “xcode-ninja” configurations that GYP can currently build.

The difference between “mostly done” and “done” exists to cover any issues we haven't yet identified :)

We do not currently plan to support full native XCode or Visual Studio generation from GN. It is theoretically possible to support such things, so we would at least look at patches adding the functionality.

## How can I help?

Check to see if your targets build under GN yet. If they don't, volunteer to help convert them!

*17 Nov 2014. We are updating the stuff we use to track progress. Watch this space and chromium-dev@ for more info!*.

## I want more info on GN!

Read these links:

- [Quick start](https://chromium.googlesource.com/chromium/src/tools/gn/+/48062805e19b4697c5fbd926dc649c78b6aaa138/docs/quick_start.md)
- [FAQ](https://chromium.googlesource.com/chromium/src/tools/gn/+/48062805e19b4697c5fbd926dc649c78b6aaa138/docs/faq.md)
- [GYP conversion cookbook](https://chromium.googlesource.com/chromium/src/tools/gn/+/48062805e19b4697c5fbd926dc649c78b6aaa138/docs/cookbook.md)
  - [Language and operation details](https://chromium.googlesource.com/chromium/src/tools/gn/+/48062805e19b4697c5fbd926dc649c78b6aaa138/docs/language.md)
- [Reference](https://chromium.googlesource.com/chromium/src/tools/gn/+/48062805e19b4697c5fbd926dc649c78b6aaa138/docs/reference.md) The built-in `gn help` documentation.
- [Style guide](https://chromium.googlesource.com/chromium/src/tools/gn/+/48062805e19b4697c5fbd926dc649c78b6aaa138/docs/style_guide.md)
- [Cross compiling and toolchains](https://chromium.googlesource.com/chromium/src/tools/gn/+/48062805e19b4697c5fbd926dc649c78b6aaa138/docs/cross_compiles.md)
- [Hacking on GN itself](https://chromium.googlesource.com/chromium/src/tools/gn/+/48062805e19b4697c5fbd926dc649c78b6aaa138/docs/hacking.md)
- [GNStandalone](https://chromium.googlesource.com/chromium/src/tools/gn/+/48062805e19b4697c5fbd926dc649c78b6aaa138/docs/standalone.md) Standalone GN projects
- [UpdateGNBinaries](https://chromium.googlesource.com/chromium/src/tools/gn/+/48062805e19b4697c5fbd926dc649c78b6aaa138/docs/update_binaries.md) Pushing new binaries



[What is GN?](https://chromium.googlesource.com/chromium/src/tools/gn/+/48062805e19b4697c5fbd926dc649c78b6aaa138/README.md)

[关于项目编译工具ninja、make、cmake的区别与优劣](https://blog.csdn.net/kangkanglhb88008/article/details/125894112)

[ninja和gn](https://zhuanlan.zhihu.com/p/136954435?utm_oi=1118087249564205056)

[OpenHarmony基线功能之编译](https://blog.51cto.com/harmonyos/5086286)
