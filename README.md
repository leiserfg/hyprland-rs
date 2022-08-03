# Hyprland-rs

A unoffical rust wrapper for Hyprland's IPC

## Getting started!

Lets get started with Hyprland-rs!

### Adding to your project

Right now the only way is to use a git submodule, this will be published to crates.to shortly!

### What this crate provides

This crate provides 3 modules (+1 for shared things)
 - [`data`][crate::data] for getting information on the compositor
 - [`event_listener`][crate::event_listener] which provides the EventListener struct for listening for events
 - [`dispatch`][crate::dispatch] for calling dispatchers and changing keywords

## Example Usage

here is an example of most of the provided features being utilized

```rust
use hyprland::data::get_monitors;
use hyprland::dispatch::{dispatch, Corner, DispatchType};
use hyprland::event_listener::EventListener;

fn main() -> std::io::Result<()> {
	// We can call dispatchers with the dispatch function!

	// Here we are telling hyprland to open kitty!
	dispatch(DispatchType::Exec("kitty".to_string()))?;

	// Here we are moving the cursor to the top left corner!
	dispatch(DispatchType::MoveCursorToCorner(Corner::TopLeft))?;

	// Here we change a keyword, yes its a dispatcher don't complain
	dispatch(DispatchType::Keyword(
		"general:border_size".to_string(),
		"30".to_string(),
	))?;

	// get all monitors
	let monitors = get_monitors();

	// and printing them all out!
	println!("{monitors:#?}");

	// Create a event listener
	let mut event_listener = EventListener::new();

	// add event, yes functions and closures both work!
	event_listener.add_workspace_change_handler(&|id| println!("workspace changed to {id:#?}"));

	// and execute the function
	// here we are using the blocking variant
	// but there is a async version too
	event_listener.start_listener_blocking()
}
```
