# metatron-x-nuklear

A single-header ANSI C gui library. 

This is a minimal state immediate mode graphical user interface toolkit written in ANSI C and licensed under public domain. It was designed as a simple
embeddable user interface for application and does not have any dependencies, a default render backend or OS window and input handling but instead provides a very modular library approach by using simple input state for input and draw commands describing primitive shapes as output. So instead of providing a
layered library that tries to abstract over a number of platform and render backends it only focuses on the actual UI.

## Gallery

![gamepad](https://cloud.githubusercontent.com/assets/8057201/14902576/339926a8-0d9c-11e6-9fee-a8b73af04473.png)
![screenshot](https://cloud.githubusercontent.com/assets/8057201/11761525/ae06f0ca-a0c6-11e5-819d-5610b25f6ef4.gif)
![screen](https://cloud.githubusercontent.com/assets/8057201/13538240/acd96876-e249-11e5-9547-5ac0b19667a0.png)
![screen2](https://cloud.githubusercontent.com/assets/8057201/13538243/b04acd4c-e249-11e5-8fd2-ad7744a5b446.png)
![node](https://cloud.githubusercontent.com/assets/8057201/9976995/e81ac04a-5ef7-11e5-872b-acd54fbeee03.gif)
![skinning](https://cloud.githubusercontent.com/assets/8057201/15991632/76494854-30b8-11e6-9555-a69840d0d50b.png)

## Features

- Immediate mode graphical user interface toolkit
- Single header library
- Written in C89 (ANSI C)
- Small codebase (~18kLOC)
- Focus on portability, efficiency and simplicity
- No dependencies (not even the standard library if not wanted)
- Fully skinnable and customizable
- Low memory footprint with total memory control if needed or wanted
- UTF-8 support
- No global or hidden state
- Customizable library modules (you can compile and use only what you need)
- Optional font baker and vertex buffer output

## Building

This library is self contained in one single header file and can be used either
in header only mode or in implementation mode. The header only mode is used
by default when included and allows including this header in other headers
and does not contain the actual implementation.

The implementation mode requires to define  the preprocessor macro
`NK_IMPLEMENTATION` in *one* .c/.cpp file before `#include`ing this file, e.g.:
```c
#define NK_IMPLEMENTATION
#include "metatronxnuklear.h"
```
IMPORTANT: Every time you include "nuklear.h" you have to define the same optional flags.
This is very important not doing it either leads to compiler errors or even worse stack corruptions.

## Example

```c
/* init gui state */
struct nk_context ctx;
nk_init_fixed(&ctx, calloc(1, MAX_MEMORY), MAX_MEMORY, &font);

enum {EASY, HARD};
static int op = EASY;
static float value = 0.6f;
static int i =  20;

if (nk_begin(&ctx, "Show", nk_rect(50, 50, 220, 220),
    NK_WINDOW_BORDER|NK_WINDOW_MOVABLE|NK_WINDOW_CLOSABLE)) {
    /* fixed widget pixel width */
    nk_layout_row_static(&ctx, 30, 80, 1);
    if (nk_button_label(&ctx, "button")) {
        /* event handling */
    }

    /* fixed widget window ratio width */
    nk_layout_row_dynamic(&ctx, 30, 2);
    if (nk_option_label(&ctx, "easy", op == EASY)) op = EASY;
    if (nk_option_label(&ctx, "hard", op == HARD)) op = HARD;

    /* custom widget pixel width */
    nk_layout_row_begin(&ctx, NK_STATIC, 30, 2);
    {
        nk_layout_row_push(&ctx, 50);
        nk_label(&ctx, "Volume:", NK_TEXT_LEFT);
        nk_layout_row_push(&ctx, 110);
        nk_slider_float(&ctx, 0, &value, 1.0f, 0.1f);
    }
    nk_layout_row_end(&ctx);
}
nk_end(&ctx);
```
![example](https://cloud.githubusercontent.com/assets/8057201/10187981/584ecd68-675c-11e5-897c-822ef534a876.png)
