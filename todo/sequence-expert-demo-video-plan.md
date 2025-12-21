# Sequence Expert Demo Video — Production Plan

## Overview
**Goal:** ~63-second video demonstrating time savings of Sequence Expert vs. manual process  
**Audience:** Hiring managers, Creative Directors, Technical Leads at Disney Digital, Tonies, Yoto, Epic  
**Core message:** Sequence Expert removes friction so creative judgment can happen in 20 seconds instead of 5 minutes

---

## Beat Sheet (60 sec)

| Time | Beat | Notes |
|------|------|-------|
| 0:00–0:02 | Slack message visible | *"Audio delivered retakes for DINO. Can you verify the pacing on seq_game05_r01_correct?"* |
| 0:02–0:04 | Split screen wipe | Labels: **MANUAL** (left) \| **SEQUENCE EXPERT** (right) |
| 0:04–0:05 | Both processes start | Simultaneous |
| 0:05–0:22 | Side by side | Manual: clicking folders, opening files, cross-referencing. SE: running. |
| 0:22–0:25 | Right side finishes | Clear "verified" state. Left side ~40% through. |
| 0:25–0:45 | Left side speeds up | 2x → 4x. Right side holds. Optional: subtle timer/clock. |
| 0:45–0:48 | Left side finishes | Both show complete. |
| 0:48–0:53 | Hold | Let the gap sit. |
| 0:53–0:57 | Second Slack message | *"Can you also verify seq_game05_r01_hint_1?"* |
| 0:57–0:60 | End card fades in | **Saurin Choksi** on maroon background (0.5s fade in, hold) |
| 0:60–0:62 | Hold | End card holds |
| 0:62–0:63 | Fade to black | 0.5s fade out |

---

## Slack Message Execution

### Decision: LOCKED
**Platform:** Slack (static window, message visible at 0:00)  
**Workspace:** StoryTink  
**Sender:** Kerry Sodorsly  
**Recipient:** Saurin (your account)

### Messages
**[0:00]** Kerry Sodorsly:  
> *"Audio delivered retakes for DINO. Can you verify the pacing on seq_game05_r01_correct?"*

**[0:53]** Kerry Sodorsly:  
> *"Can you also verify seq_game05_r01_hint_1?"*

---

## Visual Design — MANUAL Side (Left)

### The 5-Step Bounce (from case study)
The old workflow required bouncing between five applications:

| Step | Application | What's Happening |
|------|-------------|------------------|
| 1 | Script | Find the sequence reference |
| 2 | Sequences XML | Locate the sequence definition |
| 3 | Manifest | Cross-reference asset IDs to filenames |
| 4 | Finder | Navigate to the actual audio files |
| 5 | DAW | Load files, arrange in order, play |

### Execution: LOCKED
**Real screen recording.** Perform the actual 5-step manual process and record it.

Record this FIRST—you need to know the actual duration to calculate speedup timing.

---

## Visual Design — SEQUENCE EXPERT Side (Right)

### What the Viewer Sees
1. Three dropdown boxes (selecting sequence)
2. User clicks Play
3. Audio plays (3-4 files in sequence)
4. Done

### What the Viewer Hears
**The audio actually plays.** This is the proof—the tool assembled the right files, in order, and the user is listening to verify pacing. Creative judgment is happening on screen.

### "Verified" State
Since the tool's job is to *enable* the judgment (not make it), the "done" state is simply:
- Audio finishes playing
- User heard what they needed to hear
- No explicit "verified" badge needed—the silence after playback *is* the completion

### Execution: LOCKED
**Real screen recording.** Record actual Sequence Expert usage—dropdowns, play, audio plays.

---

## Production Checklist

### Pre-Production
- [ ] Choose which sequence to use (must exist in both workflows)
- [ ] Send Slack messages in StoryTink workspace (Kerry → Saurin)
- [ ] Screenshot Slack messages

### Recording
- [ ] **Record Manual process FIRST** (note actual duration: _____ min)
- [ ] Record Sequence Expert process (~20 sec)

### Post-Production
- [ ] Calculate speedup rate for Manual side (actual duration ÷ ~20 sec)
- [ ] Create split-screen edit
- [ ] Add Slack screenshot at 0:00
- [ ] Add labels (MANUAL | SEQUENCE EXPERT)
- [ ] Apply speedup to Manual side (after SE completes)
- [ ] Add second Slack message at 0:53
- [ ] Create end card (Saurin Choksi, Fraunces 700, maroon bg `#6A3843`, warm white text `#F5F3EF`)
- [ ] Add end card with fade in (0.5s), hold (2s), fade to black (0.5s)
- [ ] Export and compress

---

## End Card: LOCKED

**Option C — Name only, maroon background**

| Element | Value |
|---------|-------|
| Background | Maroon `#6A3843` |
| Text | Warm White `#F5F3EF` |
| Font | Fraunces 700, ~3rem equivalent at 1080p |
| Content | **Saurin Choksi** (name only, no role) |

### Animation Timing (per Video Editor)
- Fade IN from previous frame: 0.5s
- Hold on end card: 2 seconds minimum
- Fade OUT to black: 0.5s

Do not hold on maroon indefinitely—fade to black is the full close.

---

## Sound Design: LOCKED

**Option A: Minimal**
- Slack notification sound (0:00)
- Sequence audio plays during Sequence Expert demo (~0:15–0:22)
- Silence otherwise

The sequence audio *is* the sound design. It's proof the tool works.

---

*Last updated: December 19, 2025*
