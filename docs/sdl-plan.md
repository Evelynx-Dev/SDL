# SDL — Implementation Plan

## Structure

### sdl — root repo at `Arch/sdl/`

```
sdl/
 owl.toml # Owl project, exports via [paths].sources = "code"
 code/
 mod.mire # empty (minimal entry point)
  sdl3/
  owl.toml # exports: events, render, video, audio, clipboard, timer, version, power, filesystem, keyboard, mouse, messagebox, system, misc, cpuinfo, structs
  mod.mire # core: init, quit, get_error, delay, hints
  events.mire # poll_event, wait_event, push_event, event type constants
  render.mire # create/destroy renderer + textures, fill_rect, line, point, viewport, clip, scale, blend + color helpers + rect helpers + xywh wrappers
  video.mire # window creation, title, size, fullscreen, position, show/hide, display queries
  audio.mire # open/close device, queue/clear audio, pause/resume, gain
  clipboard.mire # set/get clipboard text and primary selection
  timer.mire # get_ticks, performance counter/frequency, delay_ns
  structs.mire # SDL3 Rect/FRect/Point/FPoint/Color alloc + get/set helpers + destroy
 sdl2/
 owl.toml # exports: events, render, video, audio, clipboard, timer
 mod.mire # core SDL2: init, quit, get_error, delay, hints
 events.mire # SDL2 PollEvent, WaitEvent, PushEvent
 render.mire # SDL2 RenderCopy (no RenderTexture), set/get viewport, clip, scale
 video.mire # SDL2 CreateWindow with x/y params, fullscreen via flags
  audio.mire # SDL2 OpenAudioDevice (capture param), QueueAudio
  clipboard.mire # SDL2 clipboard (no primary selection)
  timer.mire # SDL2 GetTicks64, GetPerformanceCounter/Frequency
  structs.mire # SDL_Rect/SDL_Point alloc + get/set helpers + destroy
 bin/
 tests/
 docs/
```

### kioto — root repo at `Arch/kioto/`
- SDL modules removed from kioto (`ext/sdl2/`, `ext/sdl3/`)
- No SDL loads in kioto's mod.mire

## Key Differences SDL3 vs SDL2

| Aspect | SDL3 | SDL2 |
|--------|------|------|
| Return type bool | `_Bool` → `:bool` | `int` → `:i64` (0 = success) |
| Texture render | `SDL_RenderTexture` | `SDL_RenderCopy` |
| Line/Point draw | `SDL_RenderLine(x1 f64, y1 f64, ...)` | `SDL_RenderDrawLine(x1 i64, y1 i64, ...)` |
| CreateWindow | `(title, w, h, flags)` | `(title, x, y, w, h, flags)` |
| SetFullscreen | `(window, bool)` | `(window, flags)` |
| ShowWindow/HideWindow | returns `:bool` | returns `void` |
| Clipboard primary | Exists | SDL 2.0.22+ maybe |
| Timer | `SDL_GetTicksNS`, `SDL_DelayNS`, `SDL_DelayPrecise` | No NS variants |
| Logical presentation | `SDL_SetRenderLogicalPresentation` | `SDL_RenderSetLogicalSize` |

## Notes
- SDL3 window appears in `hyprctl clients` only after `SDL_CreateRenderer` + `fill_screen` (first buffer commit)
- `SDL_Delay` pumps Wayland events but does NOT attach a buffer
- Incremental cache must be cleared (`rm -rf bin/.cache`) after compiler rebuild
