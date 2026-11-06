# Validation — 2026-11-06

## Scope

Six existing controller features were extracted into an independent source.
The controller-only adaptation from XR Porting Hub commit `4efb4ac` was combined
with 4uva4ek's local throw-angle patch and matching velocity-layer source/binary.
No other public patches are present in the built bundle. Framework versions:
Morphe patcher 1.13.0, build plugin 1.3.4, JDK 21, Gradle 9.6.1.

## Original inputs

| APK | Version/code | SHA-256 |
|---|---|---|
| steamlinkvr-release-beta.apk | 2.0.24/5002527 | `9f7fa542b55e71be14fd0bd925d9e04820a406e72f4feead877b9f2fc8bc0e90` |
| SL.2.0.24_5002395.apk | 2.0.24/5002395 | `605b074fed2517ffc415a0e252c24fb93dff87d88dc0943f12e5cd1c2f108dd0` |
| android-steamlinkvr-release.apk | 2.0.20/5001812 | `c696be092956854c4f5ca42ec088449a8b3e62ede72c47d4448a2d5329be6048` |

The supplied originals were read only. The 5001812 input was used for scene-code
validation; full patcher application below was run on 5002527 and 5002395.

The beta scene is 2,333,472 bytes; SHA-256
`cfa9a9081afe9f011d50088642b11c04c8c386f393231d434621121e2c65ea03`.
Its `pamir-stream-pose` action exists and its controller configuration is identical
to 5002395 (SHA-256 `36a1947073bcda038f0a61934d212bd80228b8a3e2b581dd2eba5334ce6d20d7`).
The ELF relocation/symbol audit confirms the exact beta profile, independently of
the preceding version: state `0x1089fc/0xa9c`, send `0x180fd0/0x550`, clock
`0x15f564/0x40`, state GOT `0x239b78`, submit GOT `0x239588` -> `0x163d8c`.
All three beta region hashes match the Kotlin guard and shipped native layer.

## Host verification

- Local `:patches:test`: **34 executed, 0 skipped, 0 failed** with retained real
  scene inputs for 5001812, 5002395 and 5002527. Payload SHA pins, all declared
  earlier versions, malformed-input rejection, defaults, option transitions,
  manifests and extension DEX are covered.
- `:patches:buildAndroid` and `:patches:generatePatchesList`: passed.
- Source-only checkout with freshly fetched, pinned official framework sources:
  **33 executed, 1 skipped, 0 failed**; the real-scene test correctly skips when
  no input is supplied. Compilation/packaging passed. Test verdict caching is
  disabled so an optional-input audit cannot reuse another environment's pass.
  Bundle entry bytes match the original checkout except for the framework's
  build timestamp in `META-INF/MANIFEST.MF`.
- Actual MPP load inventory: exactly the six documented public controller patches.
  The audit JVM excludes compiled production classes/resources from its parent
  classpath, so it loads the shipped archive itself.
- Full production-patcher application to both original 5002527 and 5002395:
  velocity alone; throw angle + extrapolation; pose time + velocity; HAL + grip
  haptics through Shizuku. Every combination passed execution, finalize and APK
  packaging. These temporary unsigned APKs are test evidence, not release assets.
- All original native libraries and configuration assets remain byte-identical.
  Raw-layer combinations change no existing APK entries. Shizuku changes only
  DEX packaging, the manifest and resource table while adding its controller
  libraries/manifests and helper classes.
- Canonical DEX serialization confirms all **110** original beta classes and all
  **104** original 5002395 classes remain byte-identical; no duplicate class
  definitions. Shared Shizuku code is merged once.
- Pose time plus extrapolation is correctly rejected, preserving its conflict guard.
- Reapplying throw + extrapolation adds/changes no APK entries. Reapplying the
  Shizuku pair adds no entries or classes; all **171** existing classes survive
  unchanged. The resource packager re-encodes the binary manifest; decoded XML
  remains identical except for source-line metadata.

## Runtime limits

The connected headset reports I610UEU2AZF3; its OpenXR runtime SHA-256 is
`a7dabaa1030cc7979efe987f212488c61b61e8afb9aa3add59f7ae4fbeff4c92`, matching
the retained firmware guards. This is a read-only compatibility check, not proof
that every feature is active or effective in gameplay.

No patched APK was installed or launched, no modules/settings were changed, and
no fresh beta tracking/throwing/gameplay measurement was performed. Earlier-base
runtime measurements belong to their original evidence, not this migration.
The release artifact is an inert MPP for import into Morphe.
