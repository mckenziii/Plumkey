# Plumkey ✦ a cute lil macro studio ♡

Plumkey lets you **make, save and run macros** without fighting AutoHotkey syntax.
You build macros by clicking steps together (or by recording yourself). Plumkey writes clean
**AutoHotkey v2** scripts for you, and you can export them as `.ahk` files whenever you want.

---

## Starting Plumkey

Double-click **`Plumkey.lnk`** (or `Plumkey.vbs`) in this folder. It opens in its own little window: a real
Plumkey window, not a browser tab, and it never uses Microsoft Edge.
If Plumkey is already open, that just brings its window back to the front.

* Need it on your desktop? Go to **settings → files → create desktop shortcut**.
* Closing the window doesn't stop your macros. They keep running in the background, each with a tray icon
  near the clock. To change that, turn on **settings → safety → stop macros when plumkey closes**.
* Requirements: Windows 10/11. A Plumkey installed with **PlumkeySetup.exe** brings everything else along.
  Running it straight from the source folder needs Python 3.10+ with the PySide6 package (it draws the
  window) and AutoHotkey v2.

## The setup file (PlumkeySetup.exe)

`dist\PlumkeySetup.exe` is a setup wizard with everything inside: Plumkey, its own Python, its window engine
and AutoHotkey. It works offline, needs no admin rights, and is the way to put Plumkey on another PC.

* It installs into `%LOCALAPPDATA%\Programs\Plumkey` unless you pick another folder, adds start menu and
  desktop shortcuts if you want them, and appears in Windows' list of installed apps, where it can be removed
  again. Removing keeps your macros unless you tick the box.
* Running it again over an installed Plumkey updates it and keeps your macros.
* The file isn't signed, so Windows may warn about an unknown publisher when it was downloaded:
  *More info → Run anyway* continues.
* To rebuild it after changing Plumkey: `python installer\build.py`, then `python installer\check_setup.py`
  tests the result in a scratch folder. Nothing is downloaded; the parts come from this PC's Python, PySide6
  and AutoHotkey, and none of your macros or settings go into it.

## Making a macro

Click **new macro** and pick a type:

| type | what it's for | example |
|---|---|---|
| **sequence** | a list of steps that runs when you press a hotkey | auto clicker, key spammer, anti-AFK |
| **key remap** | make one key act like another (or turn a key off) | CapsLock → Esc, media keys |
| **text expander** | type a short word and it turns into longer text | `btw` → `by the way` |
| **AHK script** | write AutoHotkey v2 code yourself, with snippets and a syntax checker | anything ✨ |

Or open **templates** and start from a ready-made one.

### Sequences

1. Add steps from the palette on the left:
   * **keyboard:** press key, type text, hold key, send keys
   * **mouse:** click, move mouse, drag, scroll
   * **flow:** wait, repeat, if color, wait for color, stop
   * **apps:** window, open program
   * **extras:** message, beep, note, custom code
2. Click a step to edit it. Drag steps to reorder them, including into and out of repeat blocks.
   Each step has an on/off switch, and **Ctrl+Z** undoes changes to the steps.
3. Set up the **trigger** card on the right. Under **start it** choose:
   * **with a hotkey**, then a **mode**:
     * **once**: press the hotkey and the steps run once.
     * **x times**: press it and the steps run a set number of times.
     * **toggle**: press to start looping, press again to stop.
     * **while held**: the steps loop while you hold the key down.
   * **as soon as it's on**: no hotkey needed. It runs **once**, **x times** or **forever** from the moment you
     switch the macro on.
4. The **code preview** shows exactly what Plumkey will run. **check** asks AutoHotkey to verify it.

Changes save automatically ("saved ♡"). Holding a hotkey down counts as one press.

### Recording

Hit **record** above the steps, then **start recording**. After the countdown, do your thing in any window,
then press **F8** (or click **■ stop** on the little pill, or **stop** in Plumkey). Plumkey turns your keys,
clicks, scrolls and pauses into steps. You can tweak them before adding them to the macro.
Anything you do in Plumkey's own window is left out of the recording.

### Picking spots and colors

Any step with a position has a **pick** button (**pick spot + color** on the color steps). Click it, then click
anywhere on your screen to grab that spot. Press **Esc** or right-click to cancel. Window steps and the
"only in a specific window" option have **pick window**, which works the same way.

## Running and stopping

* Use the **switch** on a macro card, or **run** in the editor. A little message tells you which key starts it.
* **Emergency stop:** `Ctrl + Alt + Esc` instantly quits every macro you started from Plumkey, AHK scripts
  included. It keeps working after you close the Plumkey window. You can change the key in
  **settings → safety → panic key**.
* **stop all** sits at the bottom of the sidebar.
* If you edit a macro while it's running, hit **restart to apply**.

## Exporting .ahk files

**export .ahk** saves the macro as a normal AutoHotkey script in **`My AHK Scripts\`** and shows it in
Explorer. Double-click that file to run it without Plumkey, or share it.
Have an `.ahk` file already? Use **new macro → import .ahk file…**.

## Game tips

These live in the **options** card of a macro, unless noted.

* If a game ignores your macro, set **send mode** to **Event**. Some games want slower key presses.
  "type text" steps press the real keys, so games see them too.
* If the game runs as admin, set **run as** to **uia** (no prompt) or **admin** (asks for permission each run).
* Positions are counted from the screen by default. Set **mouse spots** to **Window** if the game window moves
  around. Picked and recorded spots are then counted from the window you click in, so pick them inside the
  game window.
* Turn on **only in a specific window** (in the trigger card) to keep a macro inside your game. If you switch to
  another window it lets go of its keys and waits, then carries on when you're back. Its hotkey still stops it
  from anywhere.

## Where your stuff lives

```
Plumkey.lnk / Plumkey.vbs      start the app
My AHK Scripts\                your exported .ahk files
data\macros\                   your saved macros (one .json each)
data\scripts\                  the generated scripts Plumkey runs
data\trash\                    deleted macros (just in case ♡)
data\settings.json             your settings
data\window\                   what the Plumkey window remembers (its size and place, the welcome hint)
app\                           Plumkey itself (Python backend + web UI)
runtime\                       installed copies only: Plumkey's own Python, window engine and AutoHotkey

only in the source folder:
docs\SPEC.md                   technical design notes
docs\review\KNOWN_ISSUES.md    known problems, and which ones are fixed
installer\                     what builds the setup file
dist\PlumkeySetup.exe          the setup file
```

## Troubleshooting

* **The window doesn't open:** run `app\plumkey.py` with Python from a terminal to see the error, or check
  `data\plumkey.log` and `data\window.log`. If the PySide6 package is missing, `pip install PySide6` brings the
  window back; until then Plumkey opens in a bare Brave or Chrome window, or in your default browser.
* **"AutoHotkey not found":** install AutoHotkey v2 from <https://www.autohotkey.com/>, then click **re-detect**
  in **settings → AutoHotkey** (or **re-check** on the banner above your macros).
* **A key seems stuck after a macro:** press and release that key. Plumkey lets go of held keys when a
  macro stops. If a macro doesn't answer and has to be force-closed, Plumkey lets go of its keys itself and
  tells you which ones (for an AHK script that means Ctrl, Shift, Alt, Win and the mouse buttons).
* **A hotkey box won't take Esc:** in boxes that set a trigger, Esc cancels. Pick Esc from **more keys** instead.
