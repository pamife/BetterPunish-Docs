# Compatibility

## Supported stable releases

| Minecraft line | Verified Paper build | Required Java | Verified behavior |
| --- | --- | --- | --- |
| 1.21.11 | Paper build 132 | Java 21 | Plugin startup, enable, reload, disable, and clean shutdown |
| 26.1 | Paper 26.1.2 build 74 | Java 25 | Plugin startup, enable, reload, disable, and clean shutdown |
| 26.2 | Paper build 124 | Java 25 | Plugin startup, enable, reload, disable, and clean shutdown |

BetterPunish is a **Paper-only** plugin. It uses Paper's Adventure chat event and is not supported on a Spigot-only server.

## Paper 26.3 preview

Paper 26.3 did not have a stable build during the compatibility review. BetterPunish successfully completed the same startup and lifecycle checks on **Paper 26.3 build 15**, which Paper marked `ALPHA`, using Java 25. This is preview verification rather than a stable support guarantee; rerun the checks after Paper publishes a stable 26.3 build.

## One cross-version JAR

BetterPunish is built with Java 25 against the stable Paper 26.2 API while emitting Java 21 bytecode. Its plugin API baseline remains 1.21.11. This allows the same JAR to run on the retained Paper 1.21.11/Java 21 baseline and on Paper 26.1+/Java 25 without using NMS or version-specific implementations.

## What the matrix proves

The matrix proves that BetterPunish loads, enables, reloads, disables, and shuts down without plugin linkage or API errors on those exact builds. It does not mean every punishment, GUI gesture, chat route, external HTTP service, LuckPerms operation, or Vulcan event was interactively exercised on every version. Test those workflows on a staging server before a public release.
