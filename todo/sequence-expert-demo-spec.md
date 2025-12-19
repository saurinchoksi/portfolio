# Sequence Expert Interactive Demo — Design Spec

**Goal:** Replace the static CSS mockup in the Sequence Expert case study with an interactive demonstration that shows the tool in action, including audio playback.

**Current state:** Static placeholder mockup (lines 175-198 in `case-studies/sequence-expert.html`)

**Integration point:** Replace the `.demo-card` div in `case-studies/sequence-expert.html`

---

## Concept

A working, embedded version of Sequence Expert with sample data. Visitors actually use the dropdowns and hear real audio sequences.

**Why this is the best option:**
- Actually demonstrates the tool's value proposition
- Interactive experiences are memorable to portfolio reviewers
- Shows real implementation skill (not just a video)
- Authentic representation of the work

---

## Framing & Disclaimer

Add badge and caption:

> *"Interactive Demo — Simplified for portfolio with sample data. Original tool was a desktop-only internal application connected to Kibeam's asset pipeline."*

This framing is appropriate because:
- You're not claiming you built a responsive mobile app
- The core logic (dropdown cascade, sequence display, audio playback) is authentic
- Making it responsive for portfolio viewers demonstrates additional UX skill
- Transparency about the adaptation shows integrity

---

## Technical Approach

### 1. Data Layer

Embed sample data as JSON (no backend needed):

```javascript
const SAMPLE_DATA = {
  books: [
    {
      id: 'alphabet',
      name: 'Alphabet Fun',
      activities: [
        { id: 'letter_a', name: 'Letter A' },
        { id: 'letter_b', name: 'Letter B' }
      ]
    },
    {
      id: 'numbers',
      name: 'Number Adventures',
      activities: [
        { id: 'counting', name: 'Counting 1-10' }
      ]
    }
  ],
  sequences: {
    'letter_a': ['intro_greeting', 'vocab_apple', 'activity_trace'],
    'letter_b': ['intro_greeting', 'vocab_ball', 'activity_trace'],
    'counting': ['intro_greeting', 'count_sequence', 'celebration']
  },
  media: {
    'intro_greeting': {
      title: 'intro_greeting',
      items: [
        { type: 'audio', id: 'hello', file: 'hello.mp3', duration: 1.2 },
        { type: 'wait', duration: 0.5 },
        { type: 'audio', id: 'lets_learn', file: 'lets_learn.mp3', duration: 2.1 }
      ]
    },
    'vocab_apple': {
      title: 'vocab_apple',
      items: [
        { type: 'audio', id: 'apple_word', file: 'apple.mp3', duration: 0.8 },
        { type: 'wait', duration: 1.0 },
        { type: 'audio', id: 'apple_sentence', file: 'apple_sentence.mp3', duration: 2.5 }
      ]
    }
    // ... etc
  }
};
```

### 2. Audio Assets Needed

- 4-6 short audio clips
- Can be placeholder/royalty-free if originals are proprietary
- Suggested types: greeting, vocabulary word, encouragement, sound effect, music sting
- Keep each under 100KB, total bundle under 500KB
- Location: `assets/audio/demo/`

### 3. UI Components to Build

| Component | Description |
|-----------|-------------|
| Book dropdown | Populates on page load |
| Activity dropdown | Populates when book selected, disabled until then |
| Sequence dropdown | Populates when activity selected, disabled until then |
| Sequence display | Shows media items with icons (🔊, ⏸, 🎵) and durations |
| Play All / Stop button | Toggles playback state |
| Playing indicator | Highlights currently-playing media item |
| Progress feedback | "Playing 2 of 4..." or similar |

### 4. JavaScript Architecture

Adapt concepts from original codebase at `/Users/choksi/Perforce/choksi_wandalone/web_scripts/sequence_expert/`:

| Original Module | Adapt For Demo |
|-----------------|----------------|
| `dropdownManager.js` | Cascade population logic |
| `playbackState.js` | Track what's currently playing |
| `mediaPlaybackManager.js` | Playback queue management |

Can use simple `<audio>` element or Web Audio API for playback.

### 5. Styling Approach

- Dark theme matching the "Under the Hood" technical section
- Use portfolio design system variables:
  - `--gold` for accents and play button
  - `--maroon` for highlights
  - `--charcoal-start` / `--charcoal-end` for background gradient
- "Portfolio Demo" badge in top-right corner
- Responsive: works on mobile portfolio viewers (unlike original desktop-only tool)

---

## Files to Create

```
assets/
├── js/
│   └── sequence-expert-demo.js    # All demo logic (~200-300 lines)
└── audio/
    └── demo/
        ├── hello.mp3
        ├── lets_learn.mp3
        ├── apple.mp3
        └── ... (4-6 files total)
```

Plus additions to `assets/styles.css` or inline styles in the HTML.

---

## Effort Estimate

4-6 hours total:
- Data structure & sample content: 1 hour
- JavaScript logic: 2-3 hours
- Styling & responsive: 1-2 hours
- Audio sourcing/creation: variable

---

## Reference: Original Tool Structure

The actual Sequence Expert tool lives at:
```
/Users/choksi/Perforce/choksi_wandalone/web_scripts/sequence_expert/
```

Key files for reference:
- `dropdownManager.js` — Dropdown population and cascade logic
- `uiStateManager.js` — State management, DOM IDs, callbacks
- `playbackState.js` — Playback state tracking
- `mediaPlaybackManager.js` — Audio playback queue
- `styles.css` — Original styling (desktop-focused)
