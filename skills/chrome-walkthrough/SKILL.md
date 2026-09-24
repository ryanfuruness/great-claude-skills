---
name: "chrome-walkthrough"
description: "Teach the user to do a task themselves in a web app with a guided walkthrough overlaid in Chrome (highlights, steps, Next/Back, 'go' to have Claude do a step). Use when they ask to be shown or taught how to do something in the browser rather than have it done for them."
---

# Chrome walkthrough

Requires Claude in Chrome (the `find`, `read_page`, `javascript_tool`, `computer` and navigation tools).

When the user wants to learn a tool rather than have the task done for them, the lesson happens on the real page, with the real data, doing the real task. Claude plans the route, then puts a coaching overlay into the live tab. The overlay has a step panel, a pulsing ring around the thing to click, a tip that teaches the general pattern, and Next/Back buttons. It moves forward on its own when it sees that a step is done. The user does each step with their own hands. When they type "go" in chat, Claude does the current step for them and the walkthrough carries on. The goal is that next time they don't need the walkthrough, so every choice below comes back to what they will remember afterwards.

## Plan the route before touching the browser

Work out the task fully first, using connectors and APIs as well as the page. Find out exactly what the end state should be (which records, how many, what value) and write it down. That gives the walkthrough a checkpoint with a known answer ("you should see 13"), and it lets Claude check the result at the end against the source of truth instead of a screenshot. A walkthrough built on a guessed target teaches the tool and gets the task wrong.

Then choose the route an expert would take, not the first one that works. In most apps that means the bulk, keyboard-driven path: filter to the set, select all, apply one action. Clicking through records one at a time is not what an expert does. Teaching the slow path wastes the lesson, because the thing worth learning is the pattern that turns the next twenty similar tasks into seconds. Aim for three to six steps. Put a checkpoint step (count, confirm, compare) right before anything that changes data, so a wrong filter is caught before it spreads.

If planning turns up something the user should know (the task is framed slightly wrong, a record in the set is special, the change will trigger an automation, a view is worth saving for next time), tell them in chat before starting. If it matters on the page, also put it in the relevant step's tip.

## What makes a step teach

Each step has four parts:

- **What to do:** one action, with the keyboard shortcut shown next to the click path, e.g. `<kbd>F</kbd>`.
- **A highlighted target:** a ring around the exact element, whenever one exists.
- **How it knows you're done:** a check that detects completion, so the walkthrough moves on without being told.
- **A tip:** the general pattern behind the step. In an issue tracker, "Shift+C sets the cycle, and the same move works for priority (Shift+P), labels (L) and assignee (A)" is a lesson. "Now press Enter" is not. The tip is where most of the learning lands, so make it a pattern they can reuse, not a repeat of the instruction.

Auto-advance matters more than it seems. Without it, the user finishes a step and then has nothing to do but come back to chat and ask how to move on, which is exactly the friction the overlay is meant to remove. Detect completion wherever the page exposes it. Single-page apps usually write filter and view state into the URL (often as base64 JSON in a query parameter), and bulk actions usually show text such as "13 selected". Mark a step `manual` only when nothing reliable can be observed, and then the Next button carries it.

## Build it with the engine below

Load the Chrome tools, open a fresh tab, navigate, and wait for the app to finish loading. Find each step's target with `find` or `read_page`, and prefer stable attributes (`aria-label`, `data-*`, roles) over position or class names. Many apps reuse one label across several controls, so pick the visible match you actually mean. Then inject the engine with `javascript_tool`, filling in `STEPS` and `START`.

After injecting, always take a screenshot and look. Check that the ring is on the intended element, that the panel isn't covering what the step points at, and that every button label can be read. A ring on the wrong element teaches the wrong thing, and only a screenshot shows it. Clicks pass through the ring and the page stays usable, because only the panel captures the pointer.

```js
(() => {
  const old = window.__walk; if (old) { clearInterval(old.timer); }
  document.getElementById('walk-root')?.remove();
  const vis = e => { if (!e) return false; const r = e.getBoundingClientRect(); return r.width > 0 && r.height > 0; };
  const H = {
    q: s => [...document.querySelectorAll(s)].find(vis) || null,
    byText: (t, sel = 'button,[role=button],a,span,div') => [...document.querySelectorAll(sel)].filter(e => vis(e) && e.textContent.trim() === t).sort((a, b) => a.textContent.length - b.textContent.length || a.children.length - b.children.length)[0] || null,
    param: n => new URL(location.href).searchParams.get(n),
    b64: s => { try { return s ? atob(s.replace(/-/g, '+').replace(/_/g, '/')) : ''; } catch (e) { return ''; } },
    text: () => document.body.innerText,
  };
  // Each step: {title, body, tip?, target?: H => element|null, done?: H => bool, manual?: bool}
  const STEPS = [ /* fill in */ ];
  const START = 0;

  const root = document.createElement('div'); root.id = 'walk-root';
  root.innerHTML = `<style>
  #walk-panel{position:fixed;right:20px;bottom:20px;width:350px;z-index:2147483647;background:#fffbe6;color:#1a1a1a;border:2px solid #f5b400;border-radius:12px;box-shadow:0 10px 30px rgba(0,0,0,.45);font:14px/1.45 -apple-system,system-ui,sans-serif;padding:14px 16px}
  #walk-panel .st{font-size:11px;letter-spacing:.08em;text-transform:uppercase;color:#8a6d00;font-weight:700}
  #walk-panel h3{margin:4px 0 6px;font-size:16px;color:#1a1a1a}
  #walk-panel kbd{background:#1a1a1a;color:#fff;border-radius:4px;padding:1px 6px;font:12px ui-monospace,monospace}
  #walk-panel .tip{margin-top:8px;font-size:12.5px;color:#5a4a00;border-top:1px dashed #e0c060;padding-top:8px}
  #walk-panel .row{display:flex;gap:8px;margin-top:10px;align-items:center}
  #walk-panel button{padding:4px 12px;border-radius:6px;font:600 13px system-ui;cursor:pointer;color:#1a1a1a}
  #walk-panel .back{background:#fff;border:1px solid #c9a400}
  #walk-panel .back:disabled{color:#b8b8b8;border-color:#e2d6a8;cursor:default}
  #walk-panel .next{background:#f5b400;border:0}
  #walk-panel .hint{font-size:11.5px;color:#777}
  #walk-ring{position:fixed;z-index:2147483646;border:3px solid #f5b400;border-radius:8px;pointer-events:none;display:none;animation:walkpulse 1.2s ease-in-out infinite}
  @keyframes walkpulse{0%,100%{box-shadow:0 0 0 0 rgba(245,180,0,.7)}50%{box-shadow:0 0 0 8px rgba(245,180,0,0)}}
  </style><div id="walk-panel"></div><div id="walk-ring"></div>`;
  document.body.appendChild(root);
  const panel = root.querySelector('#walk-panel'), ring = root.querySelector('#walk-ring');
  const w = window.__walk = { steps: STEPS, idx: START, H };

  w.render = () => {
    if (w.idx >= STEPS.length) {
      panel.innerHTML = `<div class="st">Done</div><h3>That's it</h3><div>Tell me in chat and I'll verify the result.</div><div class="row"><button class="back">Back</button><button class="next">Close</button></div>`;
      panel.querySelector('.back').onclick = () => w.goto(STEPS.length - 1);
      panel.querySelector('.next').onclick = () => w.finish();
      return;
    }
    const s = STEPS[w.idx];
    panel.innerHTML = `<div class="st">Step ${w.idx + 1} of ${STEPS.length}</div><h3>${s.title}</h3><div>${s.body}</div>${s.tip ? `<div class="tip"><b>Tip:</b> ${s.tip}</div>` : ''}<div class="row"><button class="back" ${w.idx === 0 ? 'disabled' : ''}>Back</button><button class="next">${w.idx === STEPS.length - 1 ? 'Finish' : 'Next'}</button><span class="hint">${s.manual ? '' : 'Moves on by itself when done. '}Or say "go" in chat.</span></div>`;
    panel.querySelector('.back').onclick = () => w.goto(w.idx - 1);
    panel.querySelector('.next').onclick = () => w.goto(w.idx + 1);
  };
  w.goto = n => { w.idx = Math.max(0, Math.min(n, STEPS.length)); w.render(); };
  w.finish = () => { clearInterval(w.timer); root.remove(); };
  w.tick = () => {
    const s = STEPS[w.idx];
    let el = null; try { el = s && s.target ? s.target(H) : null; } catch (e) {}
    if (vis(el)) { const r = el.getBoundingClientRect(); Object.assign(ring.style, { display: 'block', left: r.left - 5 + 'px', top: r.top - 5 + 'px', width: r.width + 10 + 'px', height: r.height + 10 + 'px' }); }
    else ring.style.display = 'none';
    let ok = false; try { ok = !!(s && !s.manual && s.done && s.done(H)); } catch (e) {}
    if (ok) w.goto(w.idx + 1);
  };
  w.render();
  w.timer = setInterval(w.tick, 600);
  return { step: w.idx + 1, of: STEPS.length };
})()
```

The ring is redrawn on every tick, so it follows its target through scrolling and re-renders. Step content is HTML, so `<b>` and `<kbd>` work.

## Running it

In chat, give the user the route in a few lines: what the steps are, the number or state to expect, and anything worth knowing. Then let them drive.

- **"go":** do the current step with the browser tools, then check the step registered: its `done` fires, or for a manual step, call `__walk.goto(__walk.idx + 1)`. Say in one line what was done and which keys would have done it. "go" covers only that one step. If the step sends, publishes, deletes, or changes something other people will see, name what it will change before acting.
- **"check" or "did it work?":** read state, don't guess. Evaluate `__walk.idx`, the relevant URL parameter or page text, and take a screenshot. If they're further ahead than the overlay thinks, move it with `__walk.goto(n)`.
- **Stuck or confused:** look at the page before answering. Usually a menu is open, the focus is in the wrong place, or the app has changed since the plan. Fix the step (re-inject with the current `START`) instead of explaining around it.
- **After a full page reload** the overlay is gone, because it lives in the page. Re-inject with `START` set to the step they were on.

When the last step is done, check the end state against the source of truth from planning (API or connector, not the page), and report anything that changed unexpectedly, such as an automation that moved statuses or items that were already there. Then remove the overlay with `__walk.finish()`, and mention anything left over on the page, like a selection still active. Close the tab only if it was opened just for this and they're finished with it.

If the walkthrough taught something worth reusing (a shortcut family, a saved view, a faster route), give it one line at the end, and offer to build the same kind of walkthrough for the next task in that tool they want to learn.