# egui_inspection_tools

> **This is `egui-pop-inspection-tools`, a temporary, unchanged fork of
> [`egui_inspection_tools`](https://github.com/rerun-io/egui_crates/tree/main/crates/egui_inspection_tools)
> by [Rerun](https://rerun.io) (Lucas Meurer, Emil Ernerfeldt) and its
> contributors, published only because upstream is not on crates.io yet.**
> It is `egui_inspection_tools` 0.1.0 from
> [rerun-io/egui_crates@2d45b6d](https://github.com/rerun-io/egui_crates/tree/2d45b6d/crates/egui_inspection_tools),
> with its code unchanged: only the package name, the manifest's
> workspace-inherited fields and its crates.io dependencies (egui 0.37,
> egui_inspection 0.37) differ. All credit goes to upstream's authors; the
> licences (`LICENSE-MIT`, `LICENSE-APACHE`) are upstream's.
> This fork will be yanked once Rerun publishes `egui_inspection_tools`:
> switch back to it then.
>
> The library keeps its name, so code still uses `egui_inspection_tools::…`.
> Depend on it under the upstream name with:
>
> ```toml
> egui_inspection_tools = { package = "egui-pop-inspection-tools", version = "0.1" }
> ```

[![Latest version](https://img.shields.io/crates/v/egui_inspection_tools.svg)](https://crates.io/crates/egui_inspection_tools)
![MIT](https://img.shields.io/badge/license-MIT-blue.svg)
![Apache](https://img.shields.io/badge/license-Apache-blue.svg)

The tools that let an AI agent drive a live [egui](https://github.com/emilk/egui) app: read the AccessKit widget tree (`widget_tree`, `get_widget`, `wait_for`), take a `screenshot`, and `click`, `hover`, `scroll`, `drag`, `type_text`, `press_key`, `resize`, and `batch`.

They talk to the app over the [`egui_inspection`](https://crates.io/crates/egui_inspection) protocol, through a `Transport` you supply.
There is no MCP and no async runtime here, so the tools also run in-process and on the web (`wasm32`).
For an MCP server, use [`egui_mcp`](https://crates.io/crates/egui_mcp), which serves these same tools.

```rust,ignore
use egui_inspection_tools::{BoxFuture, call, tools};

// Your timer, for `wait_for` to poll with.
fn sleep(duration: std::time::Duration) -> BoxFuture<'static, ()> {
    Box::pin(tokio::time::sleep(duration))
}

// Hand `tools()` (name, description, JSON Schema) to your model, then run what it calls:
let output = call(&my_transport, &sleep, "click", args).await;
```
