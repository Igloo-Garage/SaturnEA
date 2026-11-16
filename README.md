# SaturnEA

**SaturnEA** is an Android-native, performance-engineered Sega Saturn emulator built upon the high-accuracy [Ymir](https://github.com/ymir-emu/Ymir) core. 

Designed from the ground up for modern ARM64 smartphones and dedicated retro gaming handhelds (such as Anbernic, Retroid, and AYN devices), SaturnEA focuses on overcoming mobile thermal throttling and TBDR GPU bottlenecks without sacrificing emulation accuracy.

---

## Core Mobile & Architectural Optimizations

* **Custom Vulkan 1.1 Rendering Pipeline:** Frame-pipelined VDP1/VDP2 rendering engineered to minimize memory bandwidth and frame-buffer overhead on mobile GPUs (specifically tuned for **ARM Mali-G52/G57/G71** and **Qualcomm Adreno** architectures).
* **Deterministic CPU Thread Scheduling:** Strict 2-big-core CPU affinity pinning and spin-wait idle-loop skipping to keep dual SH-2 synchronization stable and prevent thermal downclocking.
* **ARM64 NEON & Cache Efficiency:** Basic-block caching and SIMD-accelerated pixel/audio routines tailored for mid-range and legacy SoCs (e.g., Unisoc T820, Helio G99, Kirin 960).
* **Handheld-First Visual Enhancements:**
  * Native 2X progressive scanline rendering & **Qualcomm GSR 1.0** spatial upscaling.
  * Low-overhead, small-screen calibrated CRT shaders (Arcade Trinitron & 90s Cabinet presets).
  * Seamless VDP1 mesh-translucency smoothing.
* **Modern Quality-of-Life Features:**
  * Ultra-fast **Zstandard (`.zst`)** compressed save states.
  * Native 6-button Sega Saturn controller layout mapping (zero-latency HID support for devices like the Anbernic RG ARC series).
  * Scoped Storage (SAF) support and direct front-end launcher integration (ES-DE, Daijisho, Beacon).

---

## Performance Benchmarks & Optimization Progress

To validate real-world mobile efficiency, **SaturnEA** is continuously benchmarked on legacy, bandwidth-constrained hardware (**Huawei P10 Plus — Kirin 960 / ARM Mali-G71**) running demanding hi-res interlaced titles like *Fighters Megamix* with **GSR 1.0 upscaling, Trinitron CRT shaders, and AAudio** enabled simultaneously.

### *Fighters Megamix* — Phase 1 Optimization Impact (+134% FPS)

| Initial MVP Baseline | Phase 1 Tuned Build |
| :---: | :---: |
| ![Before Optimization](docs/images/bench_before.jpg) | ![After Optimization](docs/images/bench_after.jpg) |
| **17.9 FPS** (`52.52 ms` frame time)[cite: 1]<br>VDP2 Interlaced `704×448p` \| CPU7 @ `2096 MHz`[cite: 1] | **41.9 FPS** (`25.04 ms` frame time)[cite: 2]<br>VDP2 Half-Height `704×224p` \| CPU7 @ `2217 MHz`[cite: 2] |

**Key Engineering Gains Demonstrated:**
* **2.34× Throughput Increase:** Frame time cut in half (from `52.52 ms` down to `25.04 ms`) while keeping post-processing shaders (`GSR 1.0 + CRT`) active.
* **VDP2 Progressive Scanline Reduction:** Dynamically optimizing interlaced `704×448p` output to `704×224p` progressive scanlines dramatically reduces Mali TBDR tile-memory bandwidth overhead[cite: 1, 2].
* **Big-Core CPU Affinity & Pipelining:** Pinning dual SH-2 threads to performance cores (`CPU7` sustained at `2217 MHz`) alongside spin-wait idle-loop skipping eliminates synchronization stalls[cite: 2].


## Current Status & Roadmap

* [x] **MVP Milestone:** Core boot, dual SH-2 execution, and basic Vulkan 1.1 output verified on mobile hardware.
* [ ] **Phase 1 (In Progress):** Mali TBDR bandwidth tuning, 2-big-core affinity lock, and CD-DA audio ring-buffer stabilization.
* [ ] **Phase 2:** 2X native resolution, Qualcomm GSR 1.0 integration, and custom CRT post-processing pass.
* [ ] **Phase 3:** Public Beta release & HD texture replacement pipeline.

---

## Release Model & Early Access Policy

SaturnEA follows a dual-track release cadence:

1. **Early Access Builds (Patreon):** New experimental rendering features, device-specific SoC profiles, UI refinements, and bleeding-edge optimizations are first released to Patreon supporters for real-world hardware testing.
2. **Community Releases (Public GitHub):** Once Early Access features are stabilized and verified across target devices, the changes are merged into this public repository alongside free community APK releases (typically 4–8 weeks after initial EA testing).

*Note on GPLv3 Compliance: In strict accordance with the GNU General Public License v3.0, complete corresponding source code archives are provided directly to all Early Access recipients alongside every binary distribution.*

---

## Legal & Disclaimer

* **No BIOS or Games Included:** SaturnEA does not include any Sega Saturn BIOS files or copyrighted game ROMs. Users must dump their own legally owned console BIOS and game discs.
* Sega and Sega Saturn are registered trademarks of SEGA Corporation. This project is an independent preservation and engineering effort by **REXABYTE SOFTWARE INC.** and is not affiliated with or endorsed by SEGA.

---

## Upstream Credits & License

* **Upstream Core:** Powered by [Ymir](https://github.com/ymir-emu/Ymir), created by [StrikerX3](https://github.com/StrikerX3). Huge thanks to the upstream team for building a clean, modern, and accurate C++20 Sega Saturn architecture.
* **License:** Licensed under the **GNU General Public License v3.0 (GPLv3)**. See the `LICENSE` file for full details.