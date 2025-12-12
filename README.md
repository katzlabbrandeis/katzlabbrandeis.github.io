# Katz Lab Website

This is the website for the Katz Lab at Brandeis University.

## About

The Katz Lab studies neural ensemble dynamics of sensori-motor processes in awake rodents, combining behavior, multi-neuronal electrophysiology, complex analysis and modeling, pharmacology and optogenetics to probe ongoing spiking activity in real-time.

## Development

This website is built using Jekyll and is based on the [Allan Lab template](https://github.com/mpa139/allanlab).

### Local Development

To run the site locally:

```bash
bundle install
bundle exec jekyll serve
```

The site will be available at `http://localhost:4000`.

### Deployment

The site is automatically deployed to GitHub Pages via GitHub Actions when changes are pushed to the main branch.

## Structure

- `_data/`: Data files for team members, publications, news, etc.
- `_includes/`: Reusable HTML components
- `_layouts/`: Page layouts
- `_pages/`: Main content pages
- `images/`: Images and photos
- `blech_clust/`: Neural data analysis software documentation

## Credits

Website template from [Allan Lab](https://github.com/mpa139/allanlab).
Code released under the MIT License.
