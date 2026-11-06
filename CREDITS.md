# Attribution

The controller tracking, velocity frame, extrapolation, pose timing, grip haptics
and local throw-angle work originated with 4uva4ek and the Galaxy XR modules.
The imported Morphe integration and exact-build adaptation include code from
AngelDark92/xr-porting-hub (commit 4efb4ac), with local controller work from
4uva4ek's working copy. Source selection and original hashes are recorded in
`provenance.json`. Independence of this repository does not remove those credits.

Only the six controller features and their required support code are included.
No other XR Porting Hub patches, 4XVR patches or retired experiments are imported.

Morphe's patch framework/build plugin and patch-list generator retain their
original notices. Gradle wrapper is Apache-2.0. Shizuku API 13.1.5 (RikkaApps,
Apache-2.0) is included in the shared bridge DEX. Native layers use Khronos OpenXR
headers. Dependency code is not claimed as original controller work.

Not affiliated with Valve, Samsung, Google, Meta or the Morphe project.
