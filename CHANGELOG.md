# Changelog

## v3.2.5

### Bug Fix: Chapter pages loading only 6 images

**Problem:**
The `extractChapterImageUrls` function was extracting all `imgx.mghcdn.com` URLs from the HTML page, including preview thumbnails. That caused only 6 pages to load instead of the full chapter.

**Solution:**
Added filtering so only actual chapter page images are included:

```javascript
// Before (no filtering - grabbed all CDN URLs including thumbnails)
if (/^https:\/\/imgx\.mghcdn\.com\//i.test(candidate) && !seen.has(candidate)) {
  seen.add(candidate);
  pages.push(candidate);
}

// After (filters for actual image files and excludes thumbnails)
if (/^https:\/\/imgx\.mghcdn\.com\//i.test(candidate) && !seen.has(candidate)) {
  if (/\.(jpg|jpeg|png|webp|gif)$/i.test(candidate) && !/thumb|cover|avatar|icon|logo/i.test(candidate)) {
    seen.add(candidate);
    pages.push(candidate);
  }
}
```

**Filters applied:**
- Included: URLs ending with `.jpg`, `.jpeg`, `.png`, `.webp`, or `.gif`
- Excluded: URLs containing `thumb`, `cover`, `avatar`, `icon`, or `logo`

**File:** `Mangahub/source.js`
