# sdl — SDL2 + SDL3 bindings for Mire

Version **1.0.0** — [CHANGELOG](docs/CHANGELOG.md)

SDL bindings for the Mire language ecosystem. Exposes both SDL3 and SDL2
C APIs for video, rendering, audio, events, clipboard, and timer.

Load with `load sdl::sdl3` or `load sdl::sdl2`.

---

## sdl3

SDL3 bindings (FFI to `libSDL3.so.0`). All success-returning functions
use `:bool` (`_Bool` in C).

### sdl3::mod — Core init / quit / error / delay / hints

| Function | Returns | Description |
|----------|---------|-------------|
| `init(flags)` | `bool` | Initialize SDL subsystems |
| `init_video()` | `bool` | Initialize video subsystem |
| `quit()` | — | Shut down SDL |
| `get_error()` | `str` | Last SDL error message |
| `delay(ms)` | — | Sleep for ms |
| `get_ticks()` | `i64` | Milliseconds since SDL init |
| `get_ticks_ns()` | `i64` | Nanoseconds since SDL init |
| `set_hint(name, value)` | `bool` | Set a hint |
| `get_hint(name)` | `str` | Get a hint value |

### sdl3::events — Event polling and type constants

| Function | Returns | Description |
|----------|---------|-------------|
| `poll_event(buf)` | `bool` | Poll next event |
| `wait_event(buf)` | `bool` | Wait for next event |
| `wait_event_timeout(buf, ms)` | `bool` | Wait with timeout |
| `push_event(buf)` | `bool` | Push event onto queue |
| `flush_event(t)` | — | Flush events of type |
| `flush_events(min, max)` | — | Flush range of event types |
| `event_type(buf)` | `i64` | Read event type from buffer |
| `window_event_type(buf)` | `i64` | Read window event subtype |

Constants: `event_type_quit`, `event_type_window_event`, `event_type_key_down`,
`event_type_key_up`, `event_type_text_input`, `event_type_mouse_motion`,
`event_type_mouse_button_down`, `event_type_mouse_button_up`,
`event_type_mouse_wheel`, `event_type_finger_down`, `event_type_finger_up`,
`event_type_finger_motion`, `event_type_clipboard_update`,
`event_type_drop_file`, `event_type_drop_text`,
`event_type_audio_device_added`, `event_type_audio_device_removed`,
`event_type_display_event`, `event_type_render_device_reset`,
`event_type_user_event`.

Window event sub-types: `window_event_shown` (1) through
`window_event_destroyed` (22), plus `window_event_enter_fullscreen` (20)
and `window_event_leave_fullscreen` (21).

### sdl3::video — Window management and display queries

| Function | Returns | Description |
|----------|---------|-------------|
| `create_window(title, w, h)` | `i64` | Create a window |
| `create_window_flags(title, w, h, flags)` | `i64` | Create with flags |
| `destroy_window(win)` | — | Destroy window |
| `set_window_title(win, title)` | `bool` | Set title |
| `get_window_title(win)` | `str` | Get title |
| `set_window_size(win, w, h)` | `bool` | Set size |
| `get_window_size(win, w, h)` | `bool` | Get size |
| `set_window_pos(win, x, y)` | `bool` | Set position |
| `get_window_pos(win, x, y)` | `bool` | Get position |
| `show_window(win)` | `bool` | Show window |
| `hide_window(win)` | `bool` | Hide window |
| `maximize_window(win)` | `bool` | Maximize |
| `minimize_window(win)` | `bool` | Minimize |
| `restore_window(win)` | `bool` | Restore |
| `raise_window(win)` | `bool` | Raise to top |
| `set_fullscreen(win, fs)` | `bool` | Toggle fullscreen |
| `get_window_flags(win)` | `i64` | Current flags |
| `get_window_id(win)` | `i64` | Window ID |
| `get_window_from_id(id)` | `i64` | Window from ID |
| `get_pixel_density(win)` | `f64` | Pixel density |
| `get_display_scale(win)` | `f64` | Display scale factor |
| `get_display_count()` | `i64` | Number of displays |
| `get_primary_display()` | `i64` | Primary display ID |
| `get_display_name(id)` | `str` | Display name |
| `get_display_bounds(id, rect_ptr)` | `bool` | Display bounds |
| `get_display_usable_bounds(id, rect_ptr)` | `bool` | Usable bounds |
| `get_display_for_window(win)` | `i64` | Display for window |

Window flag constants: `window_flag_fullscreen`, `window_flag_opengl`,
`window_flag_hidden`, `window_flag_borderless`, `window_flag_resizable`,
`window_flag_minimized`, `window_flag_maximized`,
`window_flag_mouse_grabbed`, `window_flag_input_focus`,
`window_flag_mouse_focus`, `window_flag_always_on_top`,
`window_flag_utility`, `window_flag_tooltip`, `window_flag_popup_menu`,
`window_flag_vulkan`, `window_flag_transparent`,
`window_flag_high_pi_display`.

### sdl3::render — Rendering and textures

| Function | Returns | Description |
|----------|---------|-------------|
| `create_renderer(win)` | `i64` | Create renderer |
| `destroy_renderer(rend)` | — | Destroy renderer |
| `get_renderer_name(rend)` | `str` | Renderer name |
| `set_draw_color(rend, r, g, b, a)` | `bool` | Set draw color |
| `get_draw_color(rend, r, g, b, a)` | `bool` | Get draw color |
| `clear(rend)` | `bool` | Clear with draw color |
| `present(rend)` | `bool` | Present back buffer |
| `fill_screen(rend, r, g, b)` | `bool` | Fill screen with color |
| `create_texture(rend, fmt, access, w, h)` | `i64` | Create texture |
| `destroy_texture(tex)` | — | Destroy texture |
| `update_texture(tex, rect, pixels, pitch)` | `bool` | Update pixel data |
| `render_texture(rend, tex, src, dst)` | `bool` | Render texture |
| `fill_rect(rend, rect_ptr)` | `bool` | Fill rectangle |
| `draw_rect(rend, rect_ptr)` | `bool` | Draw rectangle outline |
| `draw_line(rend, x1, y1, x2, y2)` | `bool` | Draw line (f64 coords) |
| `draw_point(rend, x, y)` | `bool` | Draw point (f64 coords) |
| `set_viewport(rend, rect_ptr)` | `bool` | Set viewport |
| `get_viewport(rend, rect_ptr)` | `bool` | Get viewport |
| `viewport_set(rend)` | `bool` | Check if viewport set |
| `set_clip_rect(rend, rect_ptr)` | `bool` | Set clip rectangle |
| `get_clip_rect(rend, rect_ptr)` | `bool` | Get clip rectangle |
| `set_logical_presentation(rend, w, h, mode)` | `bool` | Set logical presentation |
| `get_logical_presentation(rend, w, h, mode)` | `bool` | Get logical presentation |
| `set_scale(rend, sx, sy)` | `bool` | Set scale |
| `get_scale(rend, sx, sy)` | `bool` | Get scale |
| `set_blend_mode(rend, mode)` | `bool` | Set blend mode |
| `get_blend_mode(rend, mode)` | `bool` | Get blend mode |
| `get_output_size(rend, w, h)` | `bool` | Output size |
| `get_current_output_size(rend, w, h)` | `bool` | Current output size |
| `set_render_target(rend, tex)` | `bool` | Set render target |
| `get_render_target(rend)` | `i64` | Get render target |

Constants: `texture_access_static`, `texture_access_streaming`,
`texture_access_target`, `pixel_format_rgba8888`, `pixel_format_bgra8888`,
`pixel_format_rgb24`, `pixel_format_bgr24`, `blend_mode_none`,
`blend_mode_blend`, `blend_mode_add`, `blend_mode_mod`, `blend_mode_mul`,
`logical_presentation_disabled`, `logical_presentation_stretch`,
`logical_presentation_letterbox`, `logical_presentation_overscan`,
`logical_presentation_integer_scale`.

### sdl3::audio — Audio playback and recording

| Function | Returns | Description |
|----------|---------|-------------|
| `get_playback_devices(count)` | `i64` | List playback devices |
| `get_recording_devices(count)` | `i64` | List recording devices |
| `get_device_name(devid)` | `str` | Device name |
| `open_device(devid, spec_ptr)` | `i64` | Open audio device |
| `close_device(devid)` | — | Close device |
| `pause_device(devid)` | `bool` | Pause playback |
| `resume_device(devid)` | `bool` | Resume playback |
| `device_paused(devid)` | `bool` | Check if paused |
| `queue_audio(devid, data, len)` | `bool` | Queue audio data |
| `get_queued_audio_size(devid)` | `i64` | Queued bytes |
| `clear_queued_audio(devid)` | — | Clear queue |
| `mix_audio(dst, src, len, volume)` | `bool` | Mix audio buffers |
| `get_device_format(devid, spec_ptr, frames)` | `bool` | Get format |
| `set_device_gain(devid, gain)` | `bool` | Set gain |
| `get_device_gain(devid)` | `f64` | Get gain |

Constants: `audio_device_default_playback` (-1), `audio_device_default_recording` (-2).

### sdl3::clipboard — Clipboard and primary selection

| Function | Returns | Description |
|----------|---------|-------------|
| `set_clipboard_text(text)` | `bool` | Set clipboard |
| `get_clipboard_text()` | `str` | Get clipboard |
| `has_clipboard_text()` | `bool` | Check clipboard |
| `set_primary_selection(text)` | `bool` | Set primary selection |
| `get_primary_selection()` | `str` | Get primary selection |
| `has_primary_selection()` | `bool` | Check primary selection |

### sdl3::timer — High-resolution timing

| Function | Returns | Description |
|----------|---------|-------------|
| `get_ticks()` | `i64` | Milliseconds since init |
| `get_ticks_ns()` | `i64` | Nanoseconds since init |
| `delay(ms)` | — | Sleep ms |
| `delay_ns(ns)` | `bool` | Sleep ns |
| `delay_precise(ns)` | — | Precise sleep ns |
| `performance_counter()` | `i64` | Performance counter |
| `performance_frequency()` | `i64` | Performance frequency |

---

## sdl2

SDL2 bindings (FFI to `libSDL2-2.0.so.0`). Success-returning functions
use `:i64` (0 = success, negative = error), matching SDL2's C `int` return.

### sdl2::mod — Core init / quit / error / delay / hints

| Function | Returns | Description |
|----------|---------|-------------|
| `init(flags)` | `i64` | Initialize subsystems |
| `init_video()` | `i64` | Initialize video |
| `quit()` | — | Shut down SDL |
| `get_error()` | `str` | Last error |
| `delay(ms)` | — | Sleep ms |
| `get_ticks()` | `i64` | Milliseconds since init |
| `set_hint(name, value)` | `bool` | Set hint |
| `get_hint(name)` | `str` | Get hint |

### sdl2::events — Event polling

| Function | Returns | Description |
|----------|---------|-------------|
| `poll_event(buf)` | `i64` | Poll next event |
| `wait_event(buf)` | `i64` | Wait for event |
| `wait_event_timeout(buf, ms)` | `i64` | Wait with timeout |
| `push_event(buf)` | `i64` | Push event |
| `flush_event(t)` | — | Flush type |
| `flush_events(min, max)` | — | Flush range |

Constants: `event_type_quit`, `event_type_window_event`, `event_type_key_down`,
`event_type_key_up`, `event_type_text_input`, `event_type_mouse_motion`,
`event_type_mouse_button_down`, `event_type_mouse_button_up`,
`event_type_mouse_wheel`, `event_type_clipboard_update`,
`event_type_drop_file`, `event_type_drop_text`,
`event_type_audio_device_added`, `event_type_audio_device_removed`,
`event_type_user_event`.

Window event sub-types: `window_event_shown` (1) through
`window_event_hit_test` (15), `window_event_iccp_changed` (16),
`window_event_display_changed` (17).

### sdl2::video — Window management

| Function | Returns | Description |
|----------|---------|-------------|
| `create_window(title, w, h)` | `i64` | Create window |
| `create_window_full(title, x, y, w, h, flags)` | `i64` | Create with position + flags |
| `destroy_window(win)` | — | Destroy window |
| `set_window_title(win, title)` | — | Set title |
| `get_window_title(win)` | `str` | Get title |
| `set_window_size(win, w, h)` | — | Set size |
| `get_window_size(win, w_ptr, h_ptr)` | — | Get size |
| `set_window_pos(win, x, y)` | — | Set position |
| `get_window_pos(win, x_ptr, y_ptr)` | — | Get position |
| `show_window(win)` | — | Show |
| `hide_window(win)` | — | Hide |
| `maximize_window(win)` | — | Maximize |
| `minimize_window(win)` | — | Minimize |
| `restore_window(win)` | — | Restore |
| `raise_window(win)` | — | Raise |
| `set_fullscreen(win, flags)` | `i64` | Set fullscreen mode |
| `get_window_flags(win)` | `i64` | Current flags |
| `get_window_id(win)` | `i64` | Window ID |
| `get_window_from_id(id)` | `i64` | Window from ID |
| `get_pixel_density(win)` | `f64` | Pixel density |
| `get_num_displays()` | `i64` | Display count |
| `get_display_name(idx)` | `str` | Display name |
| `get_display_bounds(idx, rect_ptr)` | `i64` | Display bounds |
| `get_display_usable_bounds(idx, rect_ptr)` | `i64` | Usable bounds |
| `get_desktop_display_mode(idx, mode_ptr)` | `i64` | Desktop mode |
| `get_current_display_mode(idx, mode_ptr)` | `i64` | Current mode |

### sdl2::render — Rendering

| Function | Returns | Description |
|----------|---------|-------------|
| `create_renderer(win, driver, flags)` | `i64` | Create renderer |
| `create_default_renderer(win)` | `i64` | Create default renderer |
| `destroy_renderer(rend)` | — | Destroy |
| `get_renderer_name(rend)` | `str` | Renderer name |
| `get_renderer_info(rend, info_ptr)` | `i64` | Renderer info |
| `get_num_render_drivers()` | `i64` | Driver count |
| `set_draw_color(rend, r, g, b, a)` | `i64` | Set draw color |
| `get_draw_color(rend, r, g, b, a)` | `i64` | Get draw color |
| `clear(rend)` | `i64` | Clear |
| `present(rend)` | `i64` | Present |
| `fill_screen(rend, r, g, b)` | `bool` | Fill screen with color |
| `create_texture(rend, fmt, access, w, h)` | `i64` | Create texture |
| `destroy_texture(tex)` | — | Destroy texture |
| `update_texture(tex, rect, pixels, pitch)` | `i64` | Update pixels |
| `render_copy(rend, tex, src, dst)` | `i64` | Copy texture |
| `render_copy_ex(rend, tex, src, dst, angle, center, flip)` | `i64` | Copy with transform |
| `fill_rect(rend, rect_ptr)` | `i64` | Fill rectangle |
| `draw_rect(rend, rect_ptr)` | `i64` | Draw outline |
| `draw_line(rend, x1, y1, x2, y2)` | `i64` | Draw line (i64 coords) |
| `draw_point(rend, x, y)` | `i64` | Draw point (i64 coords) |
| `set_viewport(rend, rect_ptr)` | `i64` | Set viewport |
| `get_viewport(rend, rect_ptr)` | — | Get viewport |
| `set_clip_rect(rend, rect_ptr)` | `i64` | Set clip |
| `get_clip_rect(rend, rect_ptr)` | — | Get clip |
| `set_logical_size(rend, w, h)` | `i64` | Set logical size |
| `get_logical_size(rend, w, h)` | — | Get logical size |
| `set_scale(rend, sx, sy)` | `i64` | Set scale |
| `get_scale(rend, sx, sy)` | — | Get scale |
| `set_blend_mode(rend, mode)` | `i64` | Set blend mode |
| `get_blend_mode(rend, mode)` | `i64` | Get blend mode |
| `get_output_size(rend, w, h)` | `i64` | Output size |
| `set_render_target(rend, tex)` | `i64` | Set target |
| `get_render_target(rend)` | `i64` | Get target |

### sdl2::audio — Audio playback

| Function | Returns | Description |
|----------|---------|-------------|
| `get_num_audio_devices(capture)` | `i64` | Device count |
| `get_audio_device_name(idx, capture)` | `str` | Device name |
| `open_device(device, capture, desired, obtained, allowed)` | `i64` | Open device |
| `close_device(devid)` | — | Close |
| `pause_device(devid, pause)` | — | Pause/resume |
| `queue_audio(devid, data, len)` | `i64` | Queue audio |
| `get_queued_audio_size(devid)` | `i64` | Queued bytes |
| `clear_queued_audio(devid)` | — | Clear queue |
| `mix_audio_format(dst, src, format, len, volume)` | — | Mix audio |
| `get_audio_driver(idx)` | `str` | Driver name |
| `get_current_audio_driver()` | `str` | Current driver |
| `get_audio_device_spec(idx, capture, spec_ptr)` | `i64` | Device spec |

### sdl2::clipboard

| Function | Returns | Description |
|----------|---------|-------------|
| `set_clipboard_text(text)` | `i64` | Set clipboard |
| `get_clipboard_text()` | `str` | Get clipboard |
| `has_clipboard_text()` | `i64` | Check clipboard |

### sdl2::timer

| Function | Returns | Description |
|----------|---------|-------------|
| `get_ticks()` | `i64` | Milliseconds |
| `get_ticks64()` | `i64` | 64-bit ms |
| `delay(ms)` | — | Sleep ms |
| `performance_counter()` | `i64` | Counter |
| `performance_frequency()` | `i64` | Frequency |

---

## Quick start

```mire
load kioto
load sdl::sdl3

pub fn main: () {
 if !sdl3::init_video() {
 log::error("SDL3 init failed: " + sdl3::get_error())
 return
 }

 set win = sdl3::create_window("Hello Mire" 640 480)
 set rend = sdl3::create_renderer(win)

 sdl3::fill_screen(rend 50 100 200) # first buffer commit
 sdl3::delay(3000)

 sdl3::destroy_renderer(rend)
 sdl3::destroy_window(win)
 sdl3::quit()
}
```

## Key differences: SDL3 vs SDL2

| Aspect | SDL3 | SDL2 |
|--------|------|------|
| Return type | `:bool` (`_Bool` in C) | `:i64` (`int` in C: 0 = success) |
| Texture render | `render_texture` | `render_copy` / `render_copy_ex` |
| Line/Point | `f64` coordinates | `i64` coordinates |
| CreateWindow | `(title, w, h)` | `(title, x, y, w, h, flags)` or `(title, w, h)` |
| Fullscreen | `set_fullscreen(win, bool)` | `set_fullscreen(win, flags)` |
| Show/Hide window | returns `:bool` | returns `void` |
| Clipboard primary | available | not available |
| Timer ns | `get_ticks_ns`, `delay_ns`, `delay_precise` | no ns variants |
| Logical presentation | `set_logical_presentation` | `set_logical_size` |
| Display model | display IDs (`get_primary_display`) | index-based (`get_num_displays`) |
| Audio | simplified `open_device(devid, spec)` | `open_device(device, capture, spec)` |

## Design notes

- **Wayland buffer commit**: Under Wayland, `SDL_CreateWindow` creates the
 surface but the compositor only maps it after `SDL_RenderPresent` commits
 the first buffer. Always call `fill_screen` (or `clear` + `present`) before
 waiting or polling.
- **`SDL_Delay` pumps Wayland events** but does NOT attach a buffer — `sleep+present`
 loops must present first.
- **`:bool` vs `:i64` ABI**: SDL3 C API uses `_Bool` (1 byte in `al` register),
 SDL2 uses `int` (4 bytes in `eax`). Return types must match exactly for
 correct x86_64 calling convention.
- **Event buffer**: Pass a `i64` pointer (use Mire's `rt_alloc` for SDL_Event
 struct, 128 bytes on x86_64). Event type and window event subtype read via
 `rt_read_u32` / `rt_read_u8` helpers.

## Version

**1.0.0** — See [CHANGELOG.md](docs/CHANGELOG.md).
