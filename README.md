# hugo-activity-heatmap

`hugo-activity-heatmap` is a self-contained Hugo Module that renders a GitHub-style activity overview from your Hugo pages.

It is generic by default:

- works with any Hugo section or with all regular pages
- supports `Date`, `PublishDate`, or `Lastmod`
- supports both binary and density heatmaps
- has no JavaScript and no dependency on the host site's CSS

## Hugo Directory Metadata

This repository includes the metadata expected by the Hugo themes directory:

- `theme.toml`
- root `hugo.toml` with `module.hugoVersion`
- `LICENSE`
- preview images in `images/`

## Install

Hugo Modules require a working `go` binary on your `PATH`.

If your site does not already use Hugo Modules, initialize it once from the site root:

```bash
hugo mod init github.com/your-user/your-site
```

Then add the module import to your site's `hugo.toml`:

```toml
[module]
  [[module.imports]]
    path = "github.com/kalsdjf23/hugo-activity-heatmap"
```

Optionally fetch it immediately:

```bash
hugo mod get github.com/kalsdjf23/hugo-activity-heatmap
```

## Use It

Partial:

```go-html-template
{{ partial "hugo-activity-heatmap/activity.html" (dict "page" .) }}
```

Shortcode:

```md
{{< activity-heatmap >}}
```

Common example:

```go-html-template
{{ partial "hugo-activity-heatmap/activity.html" (dict
  "page" .
  "section" "blog"
  "noun_singular" "post"
  "noun_plural" "posts"
  "show_start" true
) }}
```

## API

- `page`: required for the partial
- `section`: optional, default `""`; when empty, all regular pages are eligible
- `days`: optional, default `365`
- `title`: optional custom heading
- `mode`: optional, `density` or `binary`, default `density`
- `date_field`: optional, `Date`, `PublishDate`, or `Lastmod`, default `Date`
- `noun_singular`: optional, default `item`
- `noun_plural`: optional, default `items`
- `show_start`: optional, default `false`
- `show_start_chip`: optional, default `true`
- `show_start_text`: optional, default `true`
- `show_weekdays`: optional, default `false`; shows `Mo`, `We`, and `Fr`
- `start_label`: optional, default `Started on`
- `legend_inactive_label`: optional, binary mode only, default `No activity`
- `legend_active_label`: optional, binary mode only, default `Active day`

## Behavior

- renders the last `365` days by default
- uses a Monday-first padded grid
- scales to the width of its host container without horizontal scrolling
- shows full month labels across the top
- can optionally show `Mo`, `We`, and `Fr` labels to the left of the grid
- filters out drafts, expired pages, and future-dated pages
- groups pages by the selected date field at day resolution
- auto-generates headings such as `4 posts in the last year`
- uses your configured nouns in tooltips and `aria-label`s, for example `2 posts on Mar 4, 2026`

## Examples

Blog posts with density mode:

```md
{{< activity-heatmap section="blog" noun_singular="post" noun_plural="posts" show_weekdays="true" >}}
```

Notes with binary mode:

```md
{{< activity-heatmap
  section="notes"
  mode="binary"
  noun_singular="note"
  noun_plural="notes"
  show_start="true"
  show_start_chip="false"
  legend_inactive_label="No notes"
  legend_active_label="Published"
>}}
```

Pages based on `PublishDate`:

```md
{{< activity-heatmap
  section="blog"
  date_field="PublishDate"
  noun_singular="article"
  noun_plural="articles"
>}}
```

All regular pages:

```go-html-template
{{ partial "hugo-activity-heatmap/activity.html" (dict
  "page" .
  "noun_singular" "entry"
  "noun_plural" "entries"
) }}
```

## Local Smoke Test

This repo includes `exampleSite/` wired to the local module via a `replace` directive.

Build it with:

```bash
hugo --source exampleSite --destination /tmp/hugo-activity-heatmap-example --noBuildLock
```
