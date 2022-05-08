---
title: 'Run Jekyll in Docker'
date: 2022-05-07
tag:
    - archive
---

CLI for running Jekyll and debug my personal page locally with Docker.

I'm using an Apple Silicon M1 MBP and I failed to install Ruby with Homebrew (see [this post](https://www.earthinversion.com/blogging/how-to-install-jekyll-on-appple-m1-macbook/)). The problem was that I couldn't install RubyGems `racc` and I really don't know how to fix that. So I decided to run Jekyll as a container with [Docker](https://github.com/envygeeks/jekyll-docker/blob/master/README.md). The following commands helps me setup everything.

Initialization. Either fork this great [academicpages](https://academicpages.github.io/) template to your own github repo and clone it, or run
```bash
docker run --rm --volume="$PWD:/srv/jekyll" -it jekyll/jekyll sh -c "chown -R jekyll /usr/gem/ && jekyll new folder_name"
```

I'm using the template, and I have to add the following line to the Gemfile.

```
gem "webrick", "~>1.7"
```

Builder. 

```bash
docker run --rm --volume="$PWD:/srv/jekyll" -it jekyll/jekyll jekyll build
```

Clean. I renamed one post markdown file but it does't show up successfully. I solved this issue by doing `jekyll clean' and build it again.

```bash
docker run --rm --volume="$PWD:/srv/jekyll" -it jekyll/jekyll jekyll clean
```

Server. Setup a container name and later I can manage it in Docker dashboard.

```bash
docker run --name website --volume="$PWD:/srv/jekyll" -p 4000:4000 -it jekyll/jekyll jekyll serve --watch --incremental
```

Congrats! You shoulde be able to access your own page at `localhost:4000` by now. Enjoy!