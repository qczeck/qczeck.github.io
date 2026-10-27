# qczeck.github.io

Personal site, built by GitHub Pages with Jekyll (no theme, no plugins).

- `index.html`: home page (bio, projects, blog list)
- `_data/projects.yml`: projects, in display order
- `_posts/YYYY-MM-DD-slug.md`: blog posts, served at `/blog/slug/`
- `_layouts/`: `default` (page shell) and `post`
- `_includes/charts/`: inline SVG charts, styled by classes in the stylesheet
- `assets/css/style.css`: all styling; colours are CSS variables at the top

To preview locally: `bundle exec jekyll serve` (needs the `github-pages` gem).
