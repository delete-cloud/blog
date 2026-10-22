# Kina's Blog

Personal blog at [blog.kinaz.me](https://blog.kinaz.me), built with [OINK](https://oink.pgsty.com) (Hugo theme) and deployed to Cloudflare Pages. Chinese at `/`, English at `/en/`. Comments run on giscus via this repo's GitHub Discussions.

## Writing

Posts live in `content/blog/post/`:

- `my-post.zh.md` or `my-post.md` — Chinese (default language)
- `my-post.en.md` — English

Required front matter: `title`, `date`, `description`, `tags`. `date` must not be in the future or Hugo skips the page.

Local preview (needs Go, Hugo Extended; the module downloads on first run):

```bash
hugo server
```

Push to `main` deploys automatically through `.github/workflows/cloudflare-pages.yaml` (Hugo Extended 0.168.0 → `public/` → wrangler direct upload).

## Layout

- `hugo.yaml` — site config (title, languages, giscus, footer, logo)
- `data/home/{zh,en}.yaml` — homepage hero: eyebrow/title/social icons/buttons, avatar
- `assets/scss/_styles_project.scss` — custom overrides (rounded avatar, compact first-screen hero/footer)
- `assets/images/avatar.png`, `static/images/avatar.png` — GitHub avatar used as logo, hero image, favicon
- `content/{blog,links,about}/` — posts, friend links, about page

## License

[MIT](LICENSE) (site skeleton from OINK Starter). Post content is licensed [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/) unless noted otherwise.
