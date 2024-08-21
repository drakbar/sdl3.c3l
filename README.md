# sdl3.c3l

This binding is for [Simple DirectMedia Layer 3.0](https://wiki.libsdl.org/SDL3/FrontPage)

## Usage

### 1. Build SDL3 source
Currently (2024), SDL3 is not widely available via prebuilt binaries, thus you must [compile from source](https://wiki.libsdl.org/SDL3/Installation). Once you have the SDL3 `static` or `linked` library just drop it in the correct platform directory.


### 2. Add sdl3.c3l dependency
In your `project.json` add `sdl3` as a dependency.
```c
  "dependencies": [ "sdl3" ],
```


### 3. Use sdl3
```c
module main;

import sdl3;
import sdl3::event_type;
import sdl3::subsystem;
import sdl3::windows_flags;

fn int main(String[] args)
{
    if (sdl3::init(subsystem::VIDEO | subsystem::EVENTS) < 0)
    {
        sdl3::log_error("sdl3 init failed: %s", sdl3::get_error());
        return -1;
    }
    defer sdl3::quit();

    sdl3::Window* window = sdl3::create_window(
        "An SDL3 window", 640, 480,
        windows_flags::HIDDEN
    );
    sdl3::Renderer* renderer = sdl3::create_renderer(window, null);
    sdl3::show_window(window);

    defer sdl3::destroy_window(window);
    defer sdl3::destroy_renderer(renderer);

    bool is_running = true;

    while (is_running)
    {
        sdl3::Event e;
        if (sdl3::wait_event(&e))
        {
            switch (e.type)
            {
                case event_type::QUIT:
                    is_running = false;
            }
        }

        sdl3::set_render_draw_color(renderer, 0, 255, 0, 255);
        sdl3::render_clear(renderer);
        sdl3::render_present(renderer);
    }

    return 0;
}
```
