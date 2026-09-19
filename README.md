# Playlist Chaos — Completed Tinker

This repository contains my completed work for the AI110 Module 1 Tinker, *Playlist Chaos*. It is a Streamlit application that organizes songs into mood-based playlists such as **Hype**, **Chill**, and **Mixed**.

I used an AI coding assistant to investigate unexpected behavior, understand the relevant Python code, make focused changes, and verify each result in the running application

> Starter repository: [`ai110-module1tinker-playlistchaos-starter`](https://github.com/codepath/ai110-module1tinker-playlistchaos-starter)

## Run it

### Windows PowerShell

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
pip install -r requirements.txt
python -m streamlit run app.py
```

After starting the application, open:

[http://localhost:8501](http://localhost:8501)

## How the code is organized

- **`app.py`** — contains the Streamlit user interface, including the mood profile, add-song form, playlist tabs, search, Lucky Pick, statistics, and history.
- **`playlist_logic.py`** — contains the song classification, playlist-building, search, statistics, and Lucky Pick logic.
- **`REFLECTION.md`** — contains my reflection about debugging the application and working with an AI coding assistant.
- **`requirements.txt`** — contains the Python packages required to run the application.

## What was fixed

### 1. `search_songs` — partial and case-insensitive search

The original condition performed the substring comparison in the wrong direction:

```python
if value and value in q:
```

It checked whether the complete artist name appeared inside the shorter search query.

I corrected the condition to:

```python
if value and q in value:
```

The query and artist name are converted to lowercase, so the search is case-insensitive.

Examples:

- Searching `"AC"` finds **AC/DC**.
- Searching `"mau"` finds **deadmau5**.
- The song returned when searching `"mau"` is **Strobe**.

### 2. `compute_playlist_stats` — Hype ratio

The Hype ratio previously used the wrong denominator, causing it to display `1.00`.

It now divides the number of Hype songs by the total number of songs:

```python
total = len(all_songs)
hype_ratio = len(hype) / total if total > 0 else 0.0
```

With 11 Hype songs out of 22 total songs, the correct result is:

```text
Hype ratio: 0.50
```

### 3. `compute_playlist_stats` — average energy

Average Energy previously did not correctly represent songs from every playlist.

It now adds the energy values from all songs and divides the result by the total number of songs:

```python
total_energy = sum(song.get("energy", 0) for song in all_songs)
avg_energy = total_energy / len(all_songs)
```

The corrected application displays:

```text
Average energy: 5.73
```

## Refactor

After committing the fixes, I refactored `search_songs` by replacing the manual loop with a list comprehension:

```python
def search_songs(
    songs: List[Song],
    query: str,
    field: str = "artist",
) -> List[Song]:
    """Return songs matching the query on a given field."""
    if not query:
        return songs

    q = query.lower().strip()

    return [
        song
        for song in songs
        if q in str(song.get(field, "")).lower()
    ]
```

This refactor makes the function shorter and easier to read without changing its behavior.

## How I checked my work

I ran the Streamlit application and confirmed the following results:

1. Searching `"AC"` returns **Thunderstruck** by AC/DC.
2. Searching `"mau"` returns **Strobe** by deadmau5.
3. Search works regardless of uppercase or lowercase letters.
4. The Hype Ratio displays `0.50`.
5. Average Energy displays `5.73`.
6. Search continues working after the refactor.
7. The application runs without an error at `http://localhost:8501`.

## Git commits

The work was separated into focused commits:

```text
fix: search now matches partial, case-insensitive queries
fix: corrected playlist behavior
refactor: improved structure and readability
docs: add debugging reflection
```

## Reflection

My complete debugging reflection is available in [`REFLECTION.md`](REFLECTION.md).

The main lesson from this activity was that an AI suggestion should be treated as a hypothesis. I reproduced each problem, compared the actual behavior with the specification, made a focused change, and verified the result in the running application.

## Author

**Haida Makouangou — UNC Charlotte Graduate**  
AI110 — Intro to AI-Native Programming  
CodePath — Fall 2026

## Acknowledgments

- [CodePath](https://www.codepath.org/) for providing the AI110 course and starter project.
- The original [`Playlist Chaos starter repository`](https://github.com/codepath/ai110-module1tinker-playlistchaos-starter).
