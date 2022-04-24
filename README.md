# bevy_console

A simple half-life inspired console with support for argument parsing.

<p align="center">
  <img src="https://raw.githubusercontent.com/richodemus/bevy-console/main/doc/screenshot.png" width="100%">
</p>

## Bevy support


| bevy_console version | Supported Bevy version |
|----------------------|------------------------|
| 0.3                  | 0.6                    |
| 0.4                  | 0.7                    |

## Usage

Add `ConsolePlugin` and optionally the resource `ConsoleConfiguration`.

```rust
use bevy::prelude::*;
use bevy_console::{ConsoleConfiguration, ConsolePlugin};

fn main() {
    App::new()
        .add_plugin(ConsolePlugin)
        .insert_resource(ConsoleConfiguration {
            // override config here
            ..Default::default()
        });
}
```

Create a console command struct and system and add it to your app with `.add_console_command`.

Add [doc comments](https://doc.rust-lang.org/rust-by-example/meta/doc.html#doc-comments) to your command to provide help information in the console.

```rust
use bevy::prelude::*;
use bevy_console::{reply, AddConsoleCommand, ConsoleCommand, ConsolePlugin};

fn main() {
    App::new()
        .add_plugin(ConsolePlugin)
        .add_console_command::<ExampleCommand, _, _>(example_command);
}

/// Example command
#[derive(ConsoleCommand)]
#[console_command(name = "example")]
struct ExampleCommand {
    /// Some message
    msg: String,
}

fn example_command(mut log: ConsoleCommand<ExampleCommand>) {
    if let Some(ExampleCommand { msg }) = log.take() {
        // handle command
    }
}
```

Examples can be found in the [/examples](examples) directory.

```bash
cargo run --example log_command
```

- [log_command](/examples/log_command.rs)
- [raw_commands](/examples/raw_commands.rs)
- [write_to_console](/examples/write_to_console.rs)
- [change_console_key](/examples/change_console_key.rs)

## wasm

Should work in wasm, but you need to disable default features.
