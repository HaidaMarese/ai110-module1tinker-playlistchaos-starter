# Playlist Chaos Reflection

## Behavior I fixed

I fixed partial artist search and playlist statistics. Searching "AC" now finds AC/DC, and the statistics now calculate the Hype ratio and average energy using all songs.

## How I used AI

I gave the AI the expected behavior, actual behavior, and relevant code. I treated its explanation as a hypothesis and tested every change in the app.

## Refactor decision

I accepted a small refactor that replaced a manual loop with a list comprehension. I tested "AC" and "mau" again to confirm the behavior did not change.

## Group takeaway

A helpful debugging habit is to reproduce the problem first, give AI the expected and actual results, make one small change, and verify it before continuing.

## Special activity answer

Strobe