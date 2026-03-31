---
name: tiktok-browser
description: >
  Browse, search, and research content on TikTok using browser automation tools
  (Claude in Chrome extension). Use this skill whenever the user asks to browse TikTok,
  search for TikTok videos, find TikTok creators, research trending content on TikTok,
  monitor TikTok trends, collect TikTok video links, or analyze TikTok content.
  Also trigger when the user mentions TikTok in the context of content research,
  competitor analysis, trend monitoring, or video discovery — even if they don't
  explicitly say "browse." This skill is read-only (browsing/monitoring only, no
  posting or commenting). Requires the Claude in Chrome extension and a logged-in
  TikTok session in the browser.
---

# TikTok Browser Skill

This skill teaches you how to efficiently browse and research content on TikTok's
web interface using browser automation. It encodes hard-won knowledge about TikTok's
page structure, UI quirks, and data availability that would otherwise cost you dozens
of wasted tool calls to discover on your own.

## Prerequisites

Before starting any TikTok browsing task:

1. **Claude in Chrome extension must be active** — you need browser automation tools
   (`read_page`, `find`, `computer`, `navigate`, `form_input`). If these aren't
   available, tell the user they need the Claude in Chrome extension installed.
2. **User must be logged into TikTok** — many features (search, viewing profiles,
   seeing full engagement data) require authentication. If you land on a login wall,
   ask the user to log in manually — you cannot enter passwords.

## Critical Domain Knowledge

Read this section before doing anything on TikTok. This knowledge prevents the most
common time-wasting mistakes.

### Where Metrics Live (and Don't)

This is the single most important thing to know. TikTok shows different data on
different pages, and the agent that doesn't know this will waste 20+ tool calls
hunting for view counts in places they don't exist:

| Page Type | What You CAN See | What You CANNOT See |
|-----------|-----------------|---------------------|
| Search results grid | Likes (heart icon on thumbnails) | View counts, shares, saves |
| Video detail/overlay | Likes, comments, saves, shares | View counts |
| Creator profile page | **View counts on thumbnails**, video grid | Per-video saves/shares |
| For You feed | Likes, comments, saves, shares | View counts |

**The only place to get view counts is on a creator's profile page**, where they
appear overlaid on video thumbnails. If a user asks for videos with X+ views and
you're on search results, don't waste time looking for view data — it's not there.
Either navigate to profiles, or use the like-to-view heuristic below.

### Like-to-View Ratio Heuristic

When you can't access view counts directly (which is most of the time), estimate
views from likes. The typical ratio is **1 like ≈ 10–50 views**. So:

- 100 likes → roughly 1K–5K views
- 1K likes → roughly 10K–50K views
- 10K likes → roughly 100K–500K views

This isn't precise, but it's good enough for filtering. If a user asks for "videos
with at least 5,000 views," look for videos with 100+ likes in search results.

### AI-Generated Content Labels

TikTok displays a **"Creator labeled as AI-generated"** badge on some videos.
This appears below the hashtags on the video detail page. It's useful when
researching AI-generated content trends — look for this badge to confirm a video
is AI-made rather than guessing from visual style alone.

## Page Structures

### Global Navigation (All Pages)

Every TikTok page has a **left sidebar** with consistent navigation:

```
[TikTok logo]        → Home (/)
[Search icon]         → Opens search panel (searchbox type="button")
[Home icon]           → For You feed (/)
[Shop icon]           → TikTok Shop (/shop)
[Explore icon]        → Explore page (/explore)
[Following icon]      → Following feed (/following)
[Friends icon]        → Friends (/friends)
[LIVE icon]           → Live streams (/live)
[Messages icon]       → Messages (/messages)
[Activity icon]       → Notifications
[Upload icon]         → Upload (/tiktokstudio/upload)
[Profile icon]        → Your profile (/@username)
[More icon]           → Settings menu
```

The search icon in the sidebar is a `searchbox` element with `type="button"`. Clicking
it opens a search panel overlay — it is NOT a text input. Once the panel opens, a real
text input appears where you can type.

### Home Feed (`/`)

Full-screen vertical video player. One video fills the viewport at a time.

- **Video area**: Center of screen, with play/pause on click
- **Engagement buttons**: Right side, stacked vertically
  - `button "Like video [count] likes"`
  - `button "Read or add comments [count] comments"`
  - `button "Add to Favorites. [count] added to Favorites"`
  - `button "Share video [count] shares"`
- **Creator info**: Bottom-left (username, caption, hashtags, sound)
- **Navigation**: Up/down arrows on the right side, or scroll
- **Video progress**: `slider "Video progress"` at bottom of video
- **Sound control**: Speaker/mute icon in the video area

### Search Results (`/search?q={query}`)

Grid layout with video thumbnails. This is where you'll spend most of your time.

- **Tabs at top**: Top, Users, Videos, LIVE
  - Use the **Videos** tab (`/search/video?q={query}`) for focused video results
- **Thumbnail grid**: Each thumbnail shows:
  - Preview image (takes 2-3 seconds to load — always wait)
  - Like count (heart icon) — this is the ONLY metric visible here
  - Creator name
  - Post date
- **Clicking a thumbnail** opens a video overlay (not a new page)

Accessibility tree pattern for search results:
```
button "Watch in full screen" [ref_N]
  link [ref_N+1] href="/@username/video/videoId"
link "username's profile" [ref_N+2] href="/@username"
```

### Video Detail / Overlay

When you click a video from search results, it opens in an overlay:

- **Left side**: Video player with play/pause
- **Right side**: Creator info + comments
  - Username, display name, post date
  - Caption with clickable hashtag links (`link href="/tag/tagname"`)
  - Sound/music link
  - AI-generated label (if applicable)
  - Engagement counts: likes, comments, saves
  - Link to video (visible as text, also available as `href`)
  - Comments section with tabs: Comments, Creator videos
- **Navigation**: Up/down chevron arrows to scroll between videos
- **"Find related content"** search bar at top of overlay
- **Close button** (X) in top-left corner
- **Recommended videos**: Grid of related videos below comments

Engagement buttons in the overlay:
```
button "Like video [count] likes"
button "Read or add comments [count] comments"
button "Add to Favorites. [count] added to Favorites"
button "Share video [count] shares"
```

### Creator Profile (`/@username`)

The profile page is the only place to get view counts.

- **Header**: Avatar, display name, username
  - Follow / Message buttons
  - Following count, Followers count, Likes count
  - Bio text
- **Tabs**: Videos, Liked (some profiles also show Reposted, Favorites)
- **Sort buttons**: Latest, Popular, Oldest
- **Video grid**: Thumbnails with **view counts overlaid** (e.g., "133.5K")
  - Hovering may reveal additional info
  - Click to open video detail

Profile accessibility pattern:
```
button "Follow [display name]"
link href="/messages?u=[userId]"
tab "Videos" / tab "Liked"
button "Latest" / "Popular" / "Oldest"
```

## URL Patterns (Use These to Skip Clicks)

Navigate directly instead of clicking through the UI — it's faster and more reliable:

| Destination | URL Pattern |
|-------------|------------|
| Home feed | `https://www.tiktok.com/` |
| Search (all) | `https://www.tiktok.com/search?q={query}` |
| Search (videos only) | `https://www.tiktok.com/search/video?q={query}` |
| Creator profile | `https://www.tiktok.com/@{username}` |
| Specific video | `https://www.tiktok.com/@{username}/video/{videoId}` |
| Hashtag/tag page | `https://www.tiktok.com/tag/{tagname}` |
| Explore page | `https://www.tiktok.com/explore` |

URL-encode spaces as `%20` in search queries. Using direct navigation saves 2-4 tool
calls compared to clicking through the sidebar search.

## Common Workflows

### Searching for Videos

The most efficient approach:

1. Navigate directly to `https://www.tiktok.com/search/video?q={query}` (skip the
   sidebar search entirely)
2. Wait 3 seconds for thumbnails to load
3. Use `read_page` to get the accessibility tree — video links contain the username
   and video ID in the href
4. Click thumbnails to see engagement details in the overlay
5. Use the down chevron to browse through related videos

If the direct URL doesn't work (rare), fall back to:
1. Click the search icon (searchbox) in the left sidebar
2. Type the query in the text input that appears
3. Click the first suggestion or press Enter

### Getting View Counts

Since view counts only appear on profile pages:

1. From a video overlay or search results, note the creator's username
2. Navigate to `https://www.tiktok.com/@{username}`
3. Wait for the video grid to load (thumbnails show view counts)
4. The "Popular" sort button can help surface highest-viewed content

### Collecting Video Links

When the user wants a list of video links:

1. On the video detail/overlay page, the video URL is visible as text and in the
   accessibility tree as a link: `/@username/video/videoId`
2. The full URL follows the pattern: `https://www.tiktok.com/@{username}/video/{videoId}`
3. You can also construct the URL from the `href` attribute in the accessibility tree
4. To copy the link via TikTok's UI: click the share button → "Copy link" option

### Browsing a Creator's Content

1. Navigate to `https://www.tiktok.com/@{username}`
2. Use sort buttons (Latest / Popular / Oldest) to organize their videos
3. View counts are visible on thumbnails — no need to click into each video
4. Click a video to see its full engagement data (likes, comments, saves, shares)

## Common Pitfalls and Recovery

### "Something went wrong" on profile pages
This happens frequently. Just click the Refresh button or navigate to the URL again.

### Thumbnails not loading (black squares)
Wait 3 seconds after any navigation. If still black, scroll slightly to trigger
lazy loading, then screenshot again.

### Search bar not responding
The sidebar search icon is a `type="button"` element, not a text input. You must
click it first to open the search panel, then type in the actual input that appears.
Or better yet, just navigate directly to the search URL.

### Login walls / cookie banners
If a login modal appears, ask the user to log in. You cannot enter passwords.
Cookie banners can usually be dismissed by clicking "Decline" or the X button.

### Autoplay with sound
Videos autoplay with sound. Look for the speaker/mute icon in the video player
area to mute. The button text or tooltip says "Unmute" or "Mute" depending on state.

### Video overlay won't close
Click the X button in the top-left corner of the overlay, or press Escape.

## Tips for Efficient Browsing

1. **Prefer direct URL navigation** over clicking through menus — saves 2-4 tool calls
2. **Always wait 2-3 seconds** after navigation before taking screenshots or reading
   the page — TikTok's content loads asynchronously
3. **Use `read_page` with `filter: "interactive"`** to quickly map the page structure
   without overwhelming your context
4. **Use `find` for specific elements** like "search input" or "share button" rather
   than parsing the full accessibility tree
5. **Batch your information gathering** — get all the data you need from one page
   before navigating away, since going back may require reloading
6. **Use the Videos tab** in search results (`/search/video?q=...`) for cleaner results
   when you specifically want video content
