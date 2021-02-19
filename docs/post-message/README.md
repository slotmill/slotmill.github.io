# Slotmill Post Message API

> An API that lists available postMessage events and events dispatched by the Slotmill™ games.

## PostMessage event triggers
Events that you can dispatch to trigger various reactions in the client.

### Custom dialog events

#### API Reference


| parameter | type | default value | options | description |
|--|--|--|--|--|
| **event** | string | N/A | `"dialogOpen"`, `"dialogClose"` | The `"dialogOpen"` event opens a dialog with the according to the chosen parameters. The `"dialogClose"` event closes the current visible dialog. |
| **type** | string | `"information"` | `"information"`, `"error"` | The `"information"` dialog is recoverable and can be closed by the user or by sending a `"dialogClose"` event. While `"error"` is non recoverable and will close the game client. |
| **priority** | string | `"instant"` | `"instant"`, `"graceful"` | `"instant"` priority means that the dialog will open as soon as any animation has stopped playing. While `"graceful"` waits until a game round is over. |
| **hidden** | boolean | `false` |  | Hides the dialog window and will only show a half transparent overlay over the game, so you can open your own dialog window. |
| **title** | string | `""` |  | Title of the message |
| **message** | string | `""` |  | Message to the player |


---

#### Open dialog

> An event that can trigger different types of game dialog messages.

```
{event: 'dialogOpen', type: string, priority: string, hidden: boolean, title: string, message: string}
```


#### Different dialog examples:

##### Empty dialog

Puts a half transparent black overlay on top of the game as soon as possible, which prevents the player from interacting with the game until the empty dialog is closed.

```
{event: 'dialogOpen', hidden: true}
```

`IMPORTANT:` When triggering empty messages, you have to ensure to close the overlay by dispatching a close action event, otherwise the game will no longer be playable.

```
{event: 'dialogClose'}
```



##### Instant information dialog

Opens a message as soon as possible. Can be closed by user but also through a close event.

```
{event: 'dialogOpen', title: 'Reality check', message: 'You have played for one hour'}
```



##### Graceful information dialog

Opens a message when a game round is over. Can be closed by user but also through a close event.

```
{event: 'dialogOpen', priority: 'graceful', title: 'Reality check', message: 'You have played for one hour'}
```



##### Instant error dialog

Opens an error dialog as soon as possible. 
`IMPORTANT:` Error dialogs are non recoverable and forces the user to quit/close the game.

```
{event: 'dialogOpen', type: 'error', title: 'Error', message: 'Something went wrong'}
```

---

#### Close dialog
> An event that will close the current visible dialog in the game. Not applicable to error dialogs.

```
{event: 'dialogClose'}
```

---

#### Complete dialog example:

```
var contentWindow = document.getElementById("game_iframe").contentWindow;

openRealityCheckDialog() {  
  contentWindow.postMessage({event: 'dialogOpen', title: 'Reality check', message: 'You have played for one hour'}, "*");
}

closeDialog() {  
  contentWindow.postMessage({event: 'dialogClose'}, "*");
}
```


## Game dispatched events

These post message events are dispatched by the game client.

---

### Busy

> The Busy Event is dispatched when a game round is started.

```
{event: 'gameBusy'}
```

---

### Idle

> The Idle Event is dispatched when the game round is over and the game is waiting for user interaction.

```
{event: 'gameIdle'}
```

---

### Quit

> The Quit Event is dispatched when the game is shut down.

```
{event: 'gameQuit'}
```

---

### Event listener example: 
```
window.addEventListener("message", function (event) {
  if(event.data.event === "gameBusy"){
    // do something
  }
  if(event.data.event === "gameIdle"){
    // do something
  }
  if(event.data.event === "gameQuit"){
    // do something
  }
});
```
