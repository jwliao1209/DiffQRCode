# MeDM
This repository contains the source code for [MeDM website](https://medm2023.github.io). ([arXiv](https://arxiv.org/abs/2308.10079)).

The website leverages latest Web Technologies: [Vue](https://vuejs.org) and [Nuxt](https://nuxt.com). The former let you develop with ease, and the latter provides the best-in-class SEO to make your project spread.

## Setup

Make sure to install the dependencies:

```bash
# npm
npm install

# pnpm
pnpm install

# yarn
yarn install
```

## Development Server

Start the development server on `http://localhost:3000`:

```bash
# npm
npm run dev

# pnpm
pnpm run dev

# yarn
yarn dev
```

## Publish on GitHub Pages

We provide a workflow in `.github/workflow/deploy.yml` to let GitHub builds and deploys the website for us upon pushes to the main branch. However, if you want to deploy the website yourself without pushing to GitHub, you can use
```bash
# npm
npm run generate
npm run deploy
```

## Website License
<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/88x31.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">Creative Commons Attribution-ShareAlike 4.0 International License</a>.

