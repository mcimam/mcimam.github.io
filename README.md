
# Personal Site


This repo contain personal website source code for [mcimam.github.io](https://mcimam.github.io).

Please visit that website, I plan it to be some kind
personal blog for my own note.

Feel free to contact me &#128578;

## How to update
1. Make sure hugo is installed on the system
2. Add new content inside [content](/content/)
    or use `hugo new content [path] [flags]`
3. Make sure to publish it `draft=true`
4. check using `hugo server`
5. push to branch `main`


## Github Action
This web is deployed on github.io and build using github action.
Full github action things:
```yml
name: github pages

on:
  push:
    branches:
      - main  # Set a branch to deploy
  pull_request:

jobs:
  deploy:
    runs-on: ubuntu-20.04
    steps:
      - uses: actions/checkout@v2
        with:
          submodules: true  # Fetch Hugo themes (true OR recursive)
          fetch-depth: 0    # Fetch all history for .GitInfo and .Lastmod

      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v2
        with:
          hugo-version: 'latest'
          # extended: true

      - name: Build
        run: hugo --minify

      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        if: github.ref == 'refs/heads/main'
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./public

```


## Techstack
I use following open source project for this web:
- [Hugo](https://github.com/gohugoio/hugo)
- [PaperMod](https://github.com/adityatelange/hugo-PaperMod)

## License
Since i don't think anyone would use this other than me, i don't really mind about license. However, i just put `MIT LICENSE` here.
