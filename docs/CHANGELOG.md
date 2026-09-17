# sdl changelog

## 1.3.1 — 2026-09-17

Avenys 4.x build-manifest migration.

### Changed

- `owl.toml` now declares the full Avenys 4.x build manifest: `artifact =
  "shared"`, `runtime = "full"`, `target`, `panic = "abort"`, `incremental`,
  `debug-info`, the expanded `[paths]` (`source`/`test`/`bin`/`generated`),
  the `[cfg]` section (`publisher`, `registry`) and explicit `mire`/`kioto`
  path dependencies alongside the `sdl` self-dependency.
- `owl.lock` regenerated for the new dependency layout.

## 1.2.0 — 2026-08-03

Struct helper module + convenience wrappers for rect/point/color operations.

### New modules

- `structs` (SDL3 + SDL2): `create_rect`/`create_frect`/`create_point`/
  `create_fpoint`/`create_color` allocate SDL C structs via `rt_alloc_raw`;
  `rect_get_*`/`rect_set_*`/`frect_get_*`/`frect_set_*`/`point_get_*`/
  `point_set_*`/`fpoint_get_*`/`fpoint_set_*`/`color_get_*`/`color_set_*`
  access struct fields. `destroy(ptr)` frees (NULL-safe).
- `render` (SDL3): added `color_white`/`color_red`/`color_green`/
  `color_blue`/`color_black`/`color_clear` pre-built color structs,
  `color_destroy`/`color_get_*`/`color_set_*` accessors,
  `create_rect`/`destroy_rect`/`rect_get_*`/`rect_set_*`, and
  `fill_rect_xywh`/`draw_rect_xywh`/`render_texture_xywh`/
  `set_viewport_xywh`/`set_clip_rect_xywh` coordinate-based convenience
  wrappers that auto-allocate/free the SDL_Rect internally.
- `render` (SDL2): added `create_rect`/`destroy_rect`/`rect_get_*`/
  `rect_set_*` and `fill_rect_xywh`/`draw_rect_xywh`/`render_copy_xywh`/
  `set_viewport_xywh`/`set_clip_rect_xywh` coordinate-based wrappers.

### Tests

- `tests/structs_smoke.mire`: tests struct creation, field read/write,
  color constants, color setters, and NULL-safe destroy.

## 1.1.0 — 2026-08-02

Headless-safe module expansion + loader-compat fixes.

### New headless modules (SDL3 + SDL2)

- `version`: `get_version`/`get_revision` (+ SDL3 `major`/`minor`/`patch` decode)
- `power`: `get_power_info` + state constants
- `filesystem`: base/pref path (+ SDL3 `get_current_directory`, `get_user_folder`,
  `create_directory`, `remove_path`, `rename_path`, `copy_file`)
- `keyboard`: mod state, keycode/scancode lookups and name conversion
- `mouse`: mouse/global state, warping, capture
- `messagebox`: `show_simple_message_box` + flags
- `system`: `get_platform`, Linux thread priority
- `misc`: `open_url`
- `cpuinfo`: cores, RAM, cache line, page size, SIMD feature probes

### Fixes

- SDL2 `render`/`video`: removed SDL3-only symbols (`SDL_GetRendererName`,
  `SDL_GetWindowPixelDensity`) and renamed `SDL_GetRenderOutputSize` →
  `SDL_GetRendererOutputSize`; these made the full `sdl2` namespace fail to link.
- SDL3 `audio`: removed SDL2-only `SDL_QueueAudio`/`SDL_GetQueuedAudioSize`/
  `SDL_ClearQueuedAudio`; `SDL_MixAudio` bound with the SDL3 signature
  (`format` arg, `:bool` return).
- SDL3 `version` decode: SDL3 encodes `major*1000000 + minor*1000 + patch`
  (bit-shift decode was wrong: 3.4.12 → 45.214.108).

### Tests

- `tests/headless.mire` (sdl3) + `tests/sdl2_headless.mire` (sdl2): display-free
  modules only, pass on headless/CI.
- `tests/namespace_regression.mire`: verifies `load sdl::sdl3` exposes both the
  core `sdl3::` functions AND every sub-namespace (`video::`, `render::`,
  `timer::`, `version::`, `cpuinfo::`, …) — regression coverage for the avenys
  loader inference fix.

## 1.0.0 — 2026-07-03

Complete SDL2 and SDL3 bindings for Mire, split by functional area.

### Modules

- **SDL3** (`code/sdl3/`): init, quit, error, delay, hints, events, render,
 video, audio, clipboard, timer
- **SDL2** (`code/sdl2/`): init, quit, error, delay, hints, events, render,
 video, audio, clipboard, timer

Each module follows the SDL3 vs SDL2 API differences:
- SDL3 returns `:bool` (`_Bool` in C), SDL2 returns `:i64` (`int` in C)
- SDL3 `CreateWindow` has no x/y, SDL2 has x/y
- SDL3 uses `RenderTexture`, SDL2 uses `RenderCopy`
- SDL3 line/point use `f64`, SDL2 uses `i64`
