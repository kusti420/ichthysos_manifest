# IchthysOS — Samsung Galaxy A52s 5G (`a52sxq`)

Custom **Android 17** ROM for the Samsung Galaxy A52s 5G (SM7325), based on
**AOSP `android-17.0.0_r1`** with LineageOS device/hardware support and
crDroid-derived features.

This is the **manifest repo** — it doesn't contain source, it tells `repo`
which trees to fetch. It's cloned into `.repo/local_manifests/` on top of the
stock AOSP manifest.

## Build

```bash
mkdir ichthysos && cd ichthysos

# 1. Base AOSP manifest
repo init -u https://android.googlesource.com/platform/manifest -b android-17.0.0_r1 --git-lfs

# 2. Overlay the IchthysOS local manifests
git clone https://github.com/kusti420/ichthysos_manifest .repo/local_manifests

# 3. Sync
repo sync -c -j$(nproc --all)

# 4. Build
source build/envsetup.sh
lunch lineage_a52sxq-trunk_staging-userdebug
m otapackage
```

Output: `out/target/product/a52sxq/lineage_a52sxq-ota.zip`

Flash via a recovery that accepts self-signed OTAs (e.g. PixelOS recovery):
`adb reboot sideload` → `adb sideload <ota>.zip`.

## What's in here

| File | Purpose |
|---|---|
| `a52sxq.xml` | Device / vendor / kernel / hardware trees (forks + LineageOS deps) |
| `ichthys_apps.xml` | LineageOS FOSS apps (Jelly, Etar, Aperture, Recorder, Glimpse, Eleven) |
| `ichthys_overrides.xml` | Replaces the modified AOSP repos with `kusti420/*` forks |

All IchthysOS forks live under **`github.com/kusti420/*`** on branch
**`ichthysos-a52sxq`**. The large AOSP repos (`frameworks/base`, `system/core`,
`build/make`, `Settings`, `OmniJaws`, `crDroidSettings`) are single-commit
**snapshots** (upstream history omitted to fit GitHub limits) — they build
identically; only `git blame`/upstream log is absent on those.

Vendor blobs and the kernel are pulled from the `Galaxy-A52s-5G-AOSP` org
(read-only; unmodified by IchthysOS).
