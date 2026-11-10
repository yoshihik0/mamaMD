[日本語](README.md)

# mamaMD

**Markdown, as it is.**

mamaMD is a small viewer for reading Markdown while preserving the original text as much as possible.

It keeps markers like `#` and `**`, as well as link URLs, visible while improving line spacing and line length. It enlarges heading lines and displays images inline.

The entire app is a single file, [mamamd.html](mamamd.html), containing HTML, CSS, and JavaScript. It requires no external libraries, CDN, Node.js, build step, or API key. Keeping the source easy to inspect is a central goal.

## Concept

The name mamaMD comes from the Japanese expression **そのまま (sono mama)**, meaning “as it is.” It reflects the idea of reading Markdown with as little change to the original as possible.

The aim is a reading experience like sitting at an open counter in a sushi bar.

You can inspect the Markdown file itself, just as you can see the ingredients laid out at the counter. Like a chef making sushi right in front of you, mamaMD transforms that source text in your browser and serves it with minimal seasoning. From the raw ingredients to the finished result, keeping the process visible matters.

The writer’s markup and link destinations stay in view, with just a little help to make the text easier to read. mamaMD keeps the source and its presentation close together.

The viewer’s own workings are open to inspection, too. mamaMD is open source under the MIT License, so you can read the code, modify it, and use it on your own site.

## Usage

Place `mamamd.html` on a static web server, then open it with the URL of the Markdown you want to read appended after `?`.

It is mainly intended for reading Markdown hosted on the same site. Files on the same site can be specified with either a relative path or a full URL.

```text
https://example.com/mamamd.html?articles/hello.md
```

This fetches `https://example.com/articles/hello.md`. Relative paths are resolved from the directory where the viewer is hosted. You can also use `?url=articles/hello.md` or a root-relative path such as `/articles/hello.md`. The viewer displays the full resolved source URL at the top.

You can also specify the same file with its full URL:

```text
https://example.com/mamamd.html?https://example.com/articles/hello.md
```

The `?url=` format is also supported:

```text
https://example.com/mamamd.html?url=https%3A%2F%2Fexample.com%2Farticles%2Fhello.md
```

If the Markdown URL contains characters such as `&` or `#`, encode the entire URL with `encodeURIComponent()`. You can also put the encoded URL directly after `?`.

Markdown on external sites can also be specified using a full HTTP(S) URL, but this is not the primary use case. The source must allow CORS. Relative references that resolve to an external origin, such as `//another-site/…`, are not accepted.

## What it displays

| Item | Display and behavior |
| --- | --- |
| Body text | Wraps on a white background while preserving the original symbols, spaces, and line breaks |
| Headings | Enlarges lines starting with `#` through `######`; keeps the `#` markers |
| Links | Keeps URLs visible and clickable |
| `.md` links from the same origin | Opens the linked Markdown in mamaMD |
| External links | Adds `↗` beside the URL |
| Images from the same origin | Displays them immediately and keeps their URLs visible |
| External images | Shows the URL and `［外部画像を表示］` (“Show external image”); click the button to load the image |
| Code | Displays it as original text without making links clickable or displaying images |
| HTML | Displays it as text; tags and scripts are not interpreted or executed |

At the top of the body, the viewer displays a direct link to the source and `rendered with mamaMD`.
The browser tab displays `mamaMD:Page title`, preferring `title` in YAML front matter enclosed by `---` at the start of the file (for example, `title: "Music without a screen"`). Single-line strings are supported. If the title is empty or cannot be read, the viewer uses the first level-one heading (`# Heading`), then the filename.

“Same origin” means the same protocol, host, and port as the Markdown being read. If there is a redirect, the final Markdown URL is used as the reference. Relative links and image paths are also resolved against that URL.

The viewer handles common Markdown inline link and image syntax, as well as HTTP(S) URLs in the body text. It does not provide full Markdown rendering: reference-style links and images, tables, and emphasis are not rendered. Unrecognized syntax is left as original text.

## Network and security scope

The fetched body is inserted as text nodes, not through `innerHTML`. The source’s HTML, JavaScript, and CSS are not executed; the viewer displays content using only its own code. Link and image URLs are limited to HTTP(S), and URLs containing usernames or passwords are not used.

Automatic network requests are limited to fetching the Markdown body and images determined to be from the same origin. External images are fetched when their button is clicked, and link destinations are accessed when opened. No analytics tools or external fonts are included.

- Cookies are not sent when fetching Markdown. Images use the browser’s normal image loading, so cookies may be sent depending on browser settings.
- No Referer is sent. However, image hosts receive connection information such as your IP address.
- Whether an image is external is determined from its specified URL. If a same-origin image URL redirects externally, the browser follows that redirect.
- The viewer does not guarantee the authenticity or integrity of the original text, the accuracy of its content, or the safety of link destinations.

## If content won’t load

No CORS permission is needed when the viewer and Markdown are on the same origin. To fetch external Markdown, the source must allow CORS. Hosting the viewer on a web server does not change this restriction.

- Specify the URL of the Markdown file itself, not an HTML page. For GitHub, use the Raw URL.
- Markdown that requires cookie authentication is not supported.
- The display limit is 2 million characters.

The viewer does not include server-side conversion or a proxy. The model is to publish the original Markdown and add minimal formatting on the reader’s side.

## License

[MIT License](LICENSE)
