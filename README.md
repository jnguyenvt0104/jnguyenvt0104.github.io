# Personal blog (Jekyll + GitHub Pages)

User site (`USERNAME.github.io`) with Posts and About.

## Edit

- Site name and URL: `_config.yml` (`title`, `url`, `author.name`)
- New post: add `_posts/YYYY-MM-DD-slug.md` with `layout: post`
- About page: `about.md`
- Styles: `assets/css/style.css`

## Preview locally

Requires Ruby with Bundler:

```sh
bundle install
bundle exec jekyll serve
```

Open http://localhost:4000.

## Deploy

1. On GitHub, create a public repo named `USERNAME.github.io` (your exact username).
2. Push this folder:
   ```sh
   git init
   git add .
   git commit -m "Start blog"
   git branch -M main
   git remote add origin https://github.com/USERNAME/USERNAME.github.io.git
   git push -u origin main
   ```
3. In repo Settings → Pages, set Source to `Deploy from a branch`, Branch `main`, folder `/ (root)`.
4. Visit `https://USERNAME.github.io` after a minute or two.
