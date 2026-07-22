# sdl changelog

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
