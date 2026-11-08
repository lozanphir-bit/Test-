# Stick: Habit Checklist App Design

A habit tracker built around a home-screen checklist widget. You tick things off
("skincare", "pills") without opening the app, and the app is designed so habits
last for months, not a couple of weeks.

## 1. Core principles (why habits stick)

| Principle | How the app applies it |
|---|---|
| **Make it obvious** | The home-screen widget shows the checklist where you look 100× a day. |
| **Make it easy** | One tap to check off. No notes, ratings or forms during the daily check-in. |
| **Anchor to an existing routine** ("habit stacking") | Each habit has an *"After I…"* cue, e.g. "After I brush my teeth → skincare". |
| **Allow a tiny version** | Each habit can have a 2-minute fallback ("just sunscreen"). Doing it counts. |
| **Never miss twice** | Streaks forgive one missed day. Only two misses in a row reset them, so one bad day doesn't undo weeks of progress. |
| **Make it satisfying** | Progress ring, check animation, "all done" message, streak flame. |
| **Keep the list short** | The app nudges you toward 3–5 habits. Too many habits is the #1 reason people quit trackers. |

## 2. The widget (the key feature)

A real home-screen widget **needs a native app.** Web apps can't place
interactive widgets on iOS or Android home screens.

| Platform | Technology | Interactive checkboxes? |
|---|---|---|
| iOS 17+ | WidgetKit + App Intents (`Button(intent:)` / `Toggle(intent:)`) | ✅ Tick without opening the app |
| Android | Jetpack Glance (`AppWidget` with `actionRunCallback`) | ✅ |
| Apple Watch / Wear OS | Complications / Tiles | ✅ Nice for pills |

**Widget sizes**
- **Small:** progress ring (2/4) plus the next unchecked habit as one big tap target.
- **Medium:** up to 4 habits for the *current part of the day* (morning habits in
  the morning, evening ones at night), each with a checkbox.
- **Large:** the whole day's list, grouped Morning / Afternoon / Evening.
- **Lock screen (iOS) / At-a-glance:** "💊 not yet" or a ring.

**Widget behaviour**
- Tapping the checkbox logs the habit right away and the widget refreshes.
- Long-press or tap the name to open the app on that habit (e.g. to log "tiny").
- At midnight the widget resets (timeline entry scheduled for 00:00).
- Data is shared through an App Group (iOS) or DataStore (Android) so the app and
  widget never disagree.

**Recommended build path:** Expo / React Native for the app, plus small native
widget targets (`expo-apple-targets` for the SwiftUI widget, `react-native-android-widget`
or a Glance module for Android). This gives one shared codebase with real native
widgets. If you only use an iPhone, a pure SwiftUI app is simpler.

## 3. What's in this repo now (prototype)

`app/` is a working **installable web app (PWA)** that proves out the whole
experience before investing in native code:

- Today checklist grouped by time of day (current part of day shown first)
- One-tap check, "tiny" button, streak flame, progress ring
- Habit editor: icon, name, time of day, days of week, "After I…" cue, tiny version
- Quick-add templates (morning/evening skincare, pills, water, walk, read)
- 4-week progress heatmap plus completion %, with tiny days shown in gold
- "Never miss twice" streaks
- **Compact checklist mode** (`index.html?view=widget`): add it to your home screen
  as the closest thing to a widget a web app can offer. The installed app also has a
  long-press "Quick checklist" shortcut on Android.
- Works offline. Data stays on your device (localStorage), with export/import backup.

The data model (`habits[]` plus `log[date][habitId] = 'full' | 'tiny'`) is meant to
carry over to the native app unchanged.

## 4. More ideas (roadmap)

**For skincare and pills specifically**
- **Pill reminders that escalate:** a gentle reminder at your usual time, then another
  30 minutes later if it's still unchecked. Pills are the one habit where a
  notification is worth it.
- **Refill tracker:** enter the pill count and the app warns "~5 days left" so you
  never run out (running out is a common way medication habits break).
- **Skincare AM/PM routines as sub-steps:** cleanser → serum → moisturiser → SPF.
  The widget shows one item, and the app expands it.
- **Product rotation days:** e.g. retinol Mon/Wed/Fri, exfoliant Sat. The schedule
  already supports per-day habits.
- **Monthly progress photos** (stored privately) for skincare. Seeing results is
  strong motivation.

**For making habits last**
- **Location/time triggers:** "Arrived home → evening skincare" (iOS Shortcuts automations).
- **Weekly reflection (Sunday):** one screen: what worked, which habit felt hard → suggest
  shrinking it or moving its time.
- **"Graduate" a habit:** after ~66 days of consistency, move it to an "automatic" list so
  the daily checklist stays short.
- **Gentle copy, no guilt:** never show "You failed". Missed days say "Fresh start today".
- **Accountability buddy:** share one habit's streak with a friend (opt-in).
- **Health integrations:** Apple Health / Health Connect for water and steps, auto-ticked.

## 5. Next steps

1. Use the PWA for 1–2 weeks to check that the habit list and flow feel right.
2. Build the native app (Expo) with the iOS/Android checklist widget.
3. Add pill reminders plus the refill tracker.
4. Add the weekly reflection and "graduate" features.
