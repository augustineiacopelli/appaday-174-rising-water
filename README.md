# Rising Water

**AppADay 174** · Category G (Games) · No AI

**Live:** https://augustineiacopelli.github.io/appaday-174-rising-water/
**Portfolio:** https://augustineiacopelli.github.io/appaday/

A daily word ladder with a flood on your heels. Climb from a start word at the bottom to a goal word at the top by changing one letter at a time, with every step a real four letter word. Every move lifts the water one rung, and so does every hint. Reach the goal before the water tops the ladder.

## How it plays

Tap one of the four tiles, then tap a letter. A valid move climbs one rung. A word that is not in the list, or one already on your ladder, shakes the tiles and costs nothing. Undo steps you back down, but the water stays where it is. A hint names a next word that brings you one step closer to the goal and costs one rung of water. On a keyboard, type a letter to change the selected tile, use the arrow keys to pick a tile, Backspace to undo, and ? for a hint.

Everyone gets the same ladder each day. The day's par (the shortest route) follows the week, four on Monday and Tuesday, five on Wednesday and Thursday, six on Friday, and seven on the weekend, and the ladder is always par plus three rungs tall.

When the day's ladder is finished it locks and shows your climb, a shortest route when yours ran long, your stats, and a share line of squares (green for a step closer, blue for water spent elsewhere, white for rungs to spare, a wave if the water won). Practice ladders are unlimited after that and never touch your stats.

## How it works

The app is one self-contained `index.html` with no libraries and no Claude call. It embeds a list of 1,720 common four letter words, drawn from the ENABLE word list (so no names or abbreviations) ranked by everyday frequency, with profanity and obscure words removed.

At load it links every pair of words that differ by one letter by bucketing each word under its four wildcard patterns (for example `c_ld`), then keeps the largest connected group of words, which holds about 93 percent of the list. Daily start and goal words come only from the 1,100 most common words in that group, and never from a plain plural.

The puzzle is seeded by the local date (YYYYMMDD) through mulberry32. A start word with at least three neighbors is drawn, a breadth first search finds every common word at exactly par distance, and the goal is drawn from those, so a route of par length is guaranteed. A second search from the goal powers the hints, the progress messages, and the shortest route shown at the end.

Stats live in localStorage under `appaday.174.stats` (played, won, current and best streak, and a distribution of water used beyond par), and an unfinished daily resumes from `appaday.174.today`. Every storage access is wrapped in try/catch.

## Checks run before shipping

A node check confirmed the dictionary has 1,720 unique, well formed words, the largest component holds 92.6 percent of them, and a valid puzzle with the right par exists for every date over the next 400 days (398 distinct pairs, about 1 ms per day to generate). A jsdom test played a full daily, a loss, a hint, an undo, a rejected word, and a practice ladder, and checked the saved stats and share line. Layout was checked at 375, 390, and 1280 pixels with no horizontal scroll.

## Files

`index.html` is the whole app. Its head carries the five home screen icon tags and the two Save to Phone tags; the icon, install icons, and web manifest live in the portal repository.
