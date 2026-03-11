# TikTok Accessibility Tree Patterns

Reference document for common element patterns you'll encounter when using `read_page`
on TikTok pages. These patterns were captured from the live TikTok web app (March 2025)
and may shift as TikTok updates their UI.

## Global Sidebar (Present on All Pages)

```
link "Go to TikTok For You feed" [ref_1] href="/"
searchbox "Search" [ref_2] type="button"          ← NOT a text input, opens panel
link [ref_3] href="/"
 listitem "For You" [ref_4] type="button"
link [ref_5] href="/shop"
 listitem "Shop" [ref_6] type="button"
link [ref_7] href="/explore"
 listitem "Explore" [ref_8] type="button"
link [ref_9] href="/following"
 listitem "Following" [ref_10] type="button"
link [ref_11] href="/friends"
 listitem "Friends" [ref_12] type="button"
link [ref_13] href="/live"
 listitem "LIVE" [ref_14] type="button"
link [ref_15] href="/messages"
 listitem "Messages" [ref_16] type="button"
listitem "Activity" [ref_17] type="button"
link [ref_18] href="/tiktokstudio/upload?from=webapp"
 listitem "Upload" [ref_19] type="button"
link [ref_20] href="/@username"
 listitem "Profile" [ref_21] type="button"
listitem "More" [ref_22] type="button"
```

## Home Feed Video

```
button "Watch in full screen" [ref_N]
 generic [ref_N+1]
 link [ref_N+2] href="/@creator_username"
 button "more" [ref_N+3] type="button"
 slider "Video progress" [ref_N+4]
link [ref_N+5] href="/@creator_username"
button [ref_N+6]                                    ← Follow button
button "Like video [count] likes" [ref_N+7]
button "Read or add comments [count] comments" [ref_N+8]
button "Add to Favorites. [count] added to Favorites" [ref_N+9]
button "Share video [count] shares" [ref_N+10]
link "Watch more videos with music [sound name]" [ref_N+11] href="/music/..."
```

## Search Results Grid

Each video in the search results grid appears as:

```
button "Watch in full screen" [ref_N]
 link [ref_N+1] href="https://www.tiktok.com/@username/video/videoId"
link "username's profile" [ref_N+2] href="/@username"
```

The video link contains both the username and video ID — you can extract these
directly without clicking into the video.

## Video Detail / Overlay (from search)

When a video is opened from search results:

```
button "Watch in full screen" [ref_N]
 generic [ref_N+1]
 link [ref_N+2] href="/@creator"
 link "Watch more videos of the [tag] category" [ref_N+3] href="/tag/tagname"
 link "Watch more videos of the [tag] category" [ref_N+4] href="/tag/tagname"
 ... (more tag links)
 button "more" [ref_N+5] type="button"
 slider "Video progress" [ref_N+6]
link [ref_N+7] href="/@creator"
button [ref_N+8]                                    ← Follow button
button "Like video [count] likes" [ref_N+9]
button "Read or add comments [count] comments" [ref_N+10]
button "Add to Favorites. [count] added to Favorites" [ref_N+11]
button "Share video [count] shares" [ref_N+12]
link "Watch more videos with music [sound name]" [ref_N+13] href="/music/..."
```

Note: Hashtags appear as individual links with pattern `/tag/{tagname}`.
The video URL is also available in a visible text element showing the link.

## Creator Profile Page

```
button "Follow [display name]" [ref_N] type="button"
link [ref_N+1] href="/messages?lang=en&u=[userId]"
 button [ref_N+2] type="button"                    ← Message button
button [ref_N+3] type="button"                      ← Add friend
button "Share" [ref_N+4] type="button"
button "Actions" [ref_N+5] type="button"            ← Three-dot menu
tab [ref_N+6]                                       ← Videos tab
tab [ref_N+7]                                       ← Liked tab
button "Latest" [ref_N+8] type="button"
button "Popular" [ref_N+9] type="button"
button "Oldest" [ref_N+10] type="button"
```

Profile stats (follower count, following count, likes) are in text nodes
between the buttons — use `read_page` with `filter: "all"` to capture them.

## Search Panel (After Clicking Search Icon)

When you click the sidebar search icon, a panel opens with:
- A real text input (combobox) for typing queries
- Auto-suggest dropdown with search suggestions
- "View all results" link at the bottom

```
combobox "Search" [ref_N] type="search"             ← The actual text input
button "Search" [ref_N+1] type="submit"             ← Submit button
```

The combobox is where you type. Use `form_input` to set the value, then press Enter.
