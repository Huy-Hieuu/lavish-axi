# Resizable Conversation panel - before/after evidence

Task: let the reviewer drag the Conversation panel wider from its left edge.

Captured in Chrome at 1440x900 from a static harness, not a running server.
The harness renders the real `createChromeHtml` bar and layout markup from `src/server.js`, styles it with the real `src/chrome.css`, and runs the panel-width controller copied verbatim out of `src/chrome-client.js`.
BEFORE uses the same files from the base commit.
The chrome is dark-only, so there are no light-theme shots.

## Screenshots

- `before.png` - the base commit: fixed 360px panel.
- `after-default.png` - this change with nothing stored. Byte-identical to `before.png`: the handle is invisible until hovered.
- `after-wide.png` - after a real CDP mouse drag from the handle (x=1081) to x=700, ending over the artifact iframe. The panel is 741px, `lavish-axi:panel-width` is `741`, and a reload restores 741px.
- `after-dragging.png` - mid-drag: the 2px accent line on the panel edge.
- `after-phone.png` - phone width with a stored width of 741px. Byte-identical to the base commit at the same size: the handle is `display: none` and the sheet ignores `--panel-w`.

## Behavior checked in the harness

- Drag to x=100 clamps to 70% of the viewport (1007px at 1440 wide); drag to x=1200 clamps to the 360px default and clears the stored value.
- A real double-click on the handle resets to 360px and clears the stored value.
- ArrowLeft three times on the focused handle widens by 3 x 16px and stores it.
- A narrower window clamps the displayed width without overwriting the stored preference (stored 700, shown 630 at 900 wide).
- `body.panel-collapsed` still hides the panel and gives the artifact the full width; reopening restores the dragged width.
- Mid-drag, the artifact iframe has `pointer-events: none` and every element shows the `col-resize` cursor.
