# atripes devblog
Small jekyll based blog that serves as my notebook and general representation.
Visit blog.atrioom.com

## Usage
Pull changes with `git pull`. Add new posts in directory `_posts` as files. Add them with `git add <filename>`.
Now push to repository `git push` and push to production server (builds automatically) `git push production`.

## Start
`bundle install`
`bundle exec jekyll serve`
Also possible to start without bundle: `jekyll serve -V -d -P <port>`.

Visit at `hetzner.atrioom.com`.

## Test
Build with `jekyll build`.
You might also need a `bundle install`.
Serve with `jekyll serve`. Serves on port 4000 per default.

Uses [kramdown](https://kramdown.gettalong.org/syntax.html)
