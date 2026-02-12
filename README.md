# stachesite

Website for mmetrics.ai - a boutique AI practice exploring the intersection of AI, safety, business, and facial hair ~.~

## Local Development

**Prerequisites:** Hugo (any version, extended recommended)
```bash
# Clone with submodules
git clone --recurse-submodules https://github.com/stachemetrics/stachesite.git
cd stachesite

# Or if already cloned without submodules
git submodule update --init --recursive

# Run locally
hugo serve
```

Site runs at `http://localhost:1313`

## Structure

- `content/posts/` - Blog posts
- `content/about.md` - About page
- `content/contact.md` - Contact page
- `themes/terminal/` - Theme (git submodule)

## Deployment

GitHub Pages (coming soon)