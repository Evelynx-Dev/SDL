# sdl — SDL2 + SDL3 bindings for Mire

Version **1.2.0** — [CHANGELOG](docs/CHANGELOG.md)

SDL bindings for the Mire language ecosystem. Exposes both SDL3 and SDL2
C APIs for video, rendering, audio, events, clipboard, timer, and a set of
headless-safe system modules (version, power, filesystem, keyboard, mouse,
messagebox, system, misc, cpuinfo).

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
| `mix_audio(dst, src, format, len, volume)` | `bool` | Mix audio buffers |
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

### sdl3::version — Version info

| Function | Returns | Description |
|----------|---------|-------------|
| `get_version()` | `i64` | Encoded `major*1000000 + minor*1000 + patch` |
| `get_revision()` | `str` | Git revision string |
| `major(v)` | `i64` | Major component of an encoded version |
| `minor(v)` | `i64` | Minor component |
| `patch(v)` | `i64` | Patch component |

### sdl3::power — Power state

| Function | Returns | Description |
|----------|---------|-------------|
| `get_power_info(seconds, percent)` | `i64` | Power state (out params nullable) |

Constants: `state_unknown` (0), `state_on_battery` (1), `state_no_battery` (2),
`state_charging` (3), `state_charged` (4).

### sdl3::filesystem — Path helpers and FS operations

| Function | Returns | Description |
|----------|---------|-------------|
| `get_base_path()` | `str` | Executable base path |
| `get_pref_path(org, app)` | `str` | Per-app preference directory |
| `get_current_directory()` | `str` | CWD |
| `get_user_folder(folder)` | `str` | XDG user folder |
| `create_directory(path)` | `bool` | Create directory |
| `remove_path(path)` | `bool` | Remove file/dir |
| `rename_path(old, new)` | `bool` | Rename |
| `copy_file(old, new)` | `bool` | Copy file |

Constants: `folder_home` (0) through `folder_screenshots` (8).

### sdl3::keyboard — Keyboard and keycodes

| Function | Returns | Description |
|----------|---------|-------------|
| `has_keyboard()` | `bool` | Keyboard present |
| `get_keyboard_count()` | `i64` | Number of keyboards |
| `get_keyboard_name(id)` | `str` | Keyboard name |
| `get_mod_state()` | `i64` | Current modifier mask |
| `set_mod_state(state)` | — | Set modifiers |
| `key_from_scancode(scancode, modstate, key_event)` | `i64` | Keycode from scancode |
| `scancode_from_key(key)` | `i64` | Scancode from keycode |
| `get_scancode_name(scancode)` | `str` | Scancode name |
| `get_scancode_from_name(name)` | `i64` | Scancode from name |
| `get_key_name(key)` | `str` | Key name |
| `get_key_from_name(name)` | `i64` | Keycode from name |

Constants: `mod_none`, `mod_left_shift`, `mod_right_shift`, `mod_left_ctrl`,
`mod_right_ctrl`, `mod_left_alt`, `mod_right_alt`, `mod_shift`, `mod_ctrl`,
`mod_alt`.

### sdl3::mouse — Mouse state

| Function | Returns | Description |
|----------|---------|-------------|
| `has_mouse()` | `bool` | Mouse present |
| `get_mouse_count()` | `i64` | Number of mice |
| `get_mouse_name(id)` | `str` | Mouse name |
| `get_mouse_state(x, y)` | `i64` | Button mask (out params nullable) |
| `get_global_mouse_state(x, y)` | `i64` | Global position/buttons |
| `warp_mouse_in_window(win, x, y)` | — | Warp in window |
| `warp_mouse_global(x, y)` | `bool` | Warp globally |
| `capture_mouse(enabled)` | `bool` | Capture/relinquish |

Constants: `button_left` (1), `button_middle` (2), `button_right` (4),
`button_x1` (8), `button_x2` (16).

### sdl3::messagebox — Message boxes

| Function | Returns | Description |
|----------|---------|-------------|
| `show_simple_message_box(flags, title, message)` | `bool` | Simple message box |

Constants: `flag_error`, `flag_warning`, `flag_information`,
`flag_buttons_left_to_right`, `flag_buttons_right_to_left`,
`flag_button_returnkey_default`, `flag_button_escapekey_default`.

### sdl3::system — Platform info

| Function | Returns | Description |
|----------|---------|-------------|
| `get_platform()` | `str` | Platform name (e.g. "Linux") |
| `set_linux_thread_priority(thread_id, priority)` | `bool` | Linux thread priority |

### sdl3::misc — Misc utilities

| Function | Returns | Description |
|----------|---------|-------------|
| `open_url(url)` | `bool` | Open URL in browser |

### sdl3::cpuinfo — CPU features

| Function | Returns | Description |
|----------|---------|-------------|
| `get_num_logical_cpu_cores()` | `i64` | Logical cores |
| `get_num_system_ram()` | `i64` | RAM in MB |
| `get_cpu_cache_line_size()` | `i64` | Cache line size |
| `get_system_page_size()` | `i64` | Page size |
| `has_mmx()` / `has_sse()` / `has_sse2()` / `has_sse3()` / `has_sse41()` / `has_sse42()` | `bool` | SIMD features |
| `has_avx()` / `has_avx2()` / `has_avx512f()` / `has_neon()` | `bool` | SIMD features |

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

### sdl2::version — Version info

| Function | Returns | Description |
|----------|---------|-------------|
| `get_revision()` | `str` | Git revision string |

### sdl2::power — Power state

| Function | Returns | Description |
|----------|---------|-------------|
| `get_power_info(seconds, percent)` | `i64` | Power state |

Constants: `state_unknown` (0), `state_on_battery` (1), `state_no_battery` (2),
`state_charging` (3), `state_charged` (4).

### sdl2::filesystem — Path helpers

| Function | Returns | Description |
|----------|---------|-------------|
| `get_base_path()` | `str` | Executable base path |
| `get_pref_path(org, app)` | `str` | Per-app preference directory |

### sdl2::keyboard — Keyboard and keycodes

| Function | Returns | Description |
|----------|---------|-------------|
| `get_mod_state()` | `i64` | Current modifier mask |
| `set_mod_state(state)` | — | Set modifiers |
| `key_from_scancode(scancode)` | `i64` | Keycode from scancode |
| `scancode_from_key(key)` | `i64` | Scancode from keycode |
| `get_scancode_name(scancode)` | `str` | Scancode name |
| `get_scancode_from_name(name)` | `i64` | Scancode from name |
| `get_key_name(key)` | `str` | Key name |
| `get_key_from_name(name)` | `i64` | Keycode from name |

Constants: `mod_none`, `mod_left_shift`, `mod_right_shift`, `mod_left_ctrl`,
`mod_right_ctrl`, `mod_left_alt`, `mod_right_alt`, `mod_shift`, `mod_ctrl`,
`mod_alt`.

### sdl2::mouse — Mouse state

| Function | Returns | Description |
|----------|---------|-------------|
| `get_mouse_state(x, y)` | `i64` | Button mask |
| `get_global_mouse_state(x, y)` | `i64` | Global position/buttons |
| `warp_mouse_in_window(win, x, y)` | — | Warp in window |
| `warp_mouse_global(x, y)` | `i64` | Warp globally |
| `set_relative_mouse_mode(enabled)` | `i64` | Relative mode |
| `get_relative_mouse_mode()` | `i64` | Check relative mode |
| `capture_mouse(enabled)` | `i64` | Capture/relinquish |

Constants: `button_left` (1), `button_middle` (2), `button_right` (4),
`button_x1` (8), `button_x2` (16).

### sdl2::messagebox — Message boxes

| Function | Returns | Description |
|----------|---------|-------------|
| `show_simple_message_box(flags, title, message)` | `i64` | Simple message box |

Constants: `flag_error`, `flag_warning`, `flag_information`.

### sdl2::system — Platform info

| Function | Returns | Description |
|----------|---------|-------------|
| `get_platform()` | `str` | Platform name |
| `set_linux_thread_priority(thread_id, priority)` | `i64` | Linux thread priority |

### sdl2::misc — Misc utilities

| Function | Returns | Description |
|----------|---------|-------------|
| `open_url(url)` | `i64` | Open URL in browser |

### sdl2::cpuinfo — CPU features

| Function | Returns | Description |
|----------|---------|-------------|
| `get_cpu_count()` | `i64` | Logical cores |
| `get_num_system_ram()` | `i64` | RAM in MB |
| `get_cpu_cache_line_size()` | `i64` | Cache line size |
| `has_mmx()` / `has_sse()` / `has_sse2()` / `has_sse3()` / `has_sse41()` / `has_sse42()` | `i64` | SIMD features |
| `has_avx()` / `has_avx2()` / `has_avx512f()` / `has_neon()` | `i64` | SIMD features |

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
| Version encoding | `major*1000000 + minor*1000 + patch` | `major*1000 + minor*100 + patch` |
| Renderer name | `get_renderer_name` | via `get_renderer_info` struct |
| Pixel density | `get_pixel_density` | not available |
| FS ops | `create_directory`/`remove_path`/`rename_path`/`copy_file`/`get_current_directory` | base/pref paths only |

## Struct Helpers

SDL uses C structs like `SDL_Rect`, `SDL_FRect`, `SDL_Point`, `SDL_FPoint`,
and `SDL_Color` for many rendering functions. Since Mire passes these as raw
`i64` pointers, helper functions are provided to allocate, populate, and
manipulate these structs safely.

### sdl3::structs — Struct allocation and accessors

| Function | Returns | Description |
|----------|---------|-------------|
| `create_rect(x, y, w, h)` | `i64` | Allocate `SDL_Rect` (16 bytes) |
| `create_frect(x, y, w, h)` | `i64` | Allocate `SDL_FRect` (16 bytes, f32 fields) |
| `create_point(x, y)` | `i64` | Allocate `SDL_Point` (8 bytes) |
| `create_fpoint(x, y)` | `i64` | Allocate `SDL_FPoint` (8 bytes, f32 fields) |
| `create_color(r, g, b, a)` | `i64` | Allocate `SDL_Color` (4 bytes) |
| `destroy(ptr)` | — | Free a struct (NULL-safe) |
| `rect_get_x/y/w/h(ptr)` | `i64` | Read rect fields |
| `rect_set_x/y/w/h(ptr, val)` | — | Write rect fields |
| `frect_get_x/y/w/h(ptr)` | `f64` | Read frect fields |
| `frect_set_x/y/w/h(ptr, val)` | — | Write frect fields |
| `point_get_x/y(ptr)` | `i64` | Read point fields |
| `point_set_x/y(ptr, val)` | — | Write point fields |
| `fpoint_get_x/y(ptr)` | `f64` | Read fpoint fields |
| `fpoint_set_x/y(ptr, val)` | — | Write fpoint fields |
| `color_get_r/g/b/a(ptr)` | `i64` | Read color fields |
| `color_set_r/g/b/a(ptr, val)` | — | Write color fields |

### sdl3::render — Color and rect convenience helpers

| Function | Returns | Description |
|----------|---------|-------------|
| `color_white()` | `i64` | Pre-built white color struct |
| `color_red()` / `color_green()` / `color_blue()` | `i64` | Pre-built color structs |
| `color_black()` | `i64` | Pre-built opaque black color struct |
| `color_clear()` | `i64` | Pre-built transparent color struct |
| `color_destroy(ptr)` | — | Free a color struct |
| `color_get_r/g/b/a(ptr)` | `i64` | Read color fields |
| `color_set_r/g/b/a(ptr, val)` | — | Write color fields |
| `create_rect(x, y, w, h)` | `i64` | Allocate rect (also in structs::) |
| `destroy_rect(ptr)` | — | Free a rect (NULL-safe) |
| `rect_get_x/y/w/h(ptr)` | `i64` | Read rect fields |
| `rect_set_x/y/w/h(ptr, val)` | — | Write rect fields |
| `fill_rect_xywh(rend, x, y, w, h)` | `bool` | Fill rect by coordinates |
| `draw_rect_xywh(rend, x, y, w, h)` | `bool` | Draw rect outline by coordinates |
| `render_texture_xywh(rend, tex, sx, sy, sw, sh, dx, dy, dw, dh)` | `bool` | Render texture with source/dest rects by coords |
| `set_viewport_xywh(rend, x, y, w, h)` | `bool` | Set viewport by coordinates |
| `set_clip_rect_xywh(rend, x, y, w, h)` | `bool` | Set clip rect by coordinates |

### sdl2::structs — SDL2 struct helpers

Same as SDL3 but only `create_rect`/`destroy_rect`/`rect_get_*`/`rect_set_*`
and `create_point`/`point_get_*`/`point_set_*` (SDL2 uses `SDL_Rect`/`SDL_Point` only).

### sdl2::render — SDL2 render struct helpers

| Function | Returns | Description |
|----------|---------|-------------|
| `create_rect(x, y, w, h)` | `i64` | Allocate rect |
| `destroy_rect(ptr)` | — | Free a rect (NULL-safe) |
| `rect_get_x/y/w/h(ptr)` | `i64` | Read rect fields |
| `rect_set_x/y/w/h(ptr, val)` | — | Write rect fields |
| `fill_rect_xywh(rend, x, y, w, h)` | `i64` | Fill rect by coordinates |
| `draw_rect_xywh(rend, x, y, w, h)` | `i64` | Draw rect outline by coordinates |
| `render_copy_xywh(rend, tex, sx, sy, sw, sh, dx, dy, dw, dh)` | `i64` | Copy texture with rects by coords |
| `set_viewport_xywh(rend, x, y, w, h)` | `i64` | Set viewport by coordinates |
| `set_clip_rect_xywh(rend, x, y, w, h)` | `i64` | Set clip rect by coordinates |

## Headless testing

The `tests/headless.mire` (sdl3) and `tests/sdl2_headless.mire` (sdl2) files
exercise only modules that work without a display server: version, power,
filesystem, cpuinfo, keyboard name lookups, and system platform. They pass in
CI and on headless servers. Window/render/audio/event tests require a desktop
session and are kept out of the default suite.

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

**1.2.0** — See [CHANGELOG.md](docs/CHANGELOG.md).
