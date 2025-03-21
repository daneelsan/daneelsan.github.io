# How this works

https://daneelsan.github.io

NOTE: This is a really rough README

## Create a post

1. Create a file under `_posts/` with the format `YYYY-MM-DD-title.[html|md]`, e.g. `2023-07-18-hello-world.md`.
2. Add frontmatter to the file, e.g.:

	```
	---
	layout: post
	---
	```
3. After the frontmatter, write the post using either plain Markdown or HTML (or both).
4. To work with images, save them in the `images/` directory and link them with HTML:

	```
	<div style="text-align: center;">
		<img src="{{ site.url }}/images/<YOUR_IMAGE_NAME>.png" width="50%"/>
	</div>
	<br>
	```

## Host site locally

To serve this site locally run:
```shell
jekyll serve
```

To include the draft posts stored under `_drafts`, run:
```shell
jekyll serve --draft
```

Both serve the site at `http://localhost:4000`.

## Host site on GithubPages

There is no need to build anything.
Simply add the posts to the `_posts` directory and do a `git add <...>` and `git push`.
They should appear in https://daneelsan.github.io (it may take a few minutes to update after pushing the changes).

## Requirements

* See https://jekyllrb.com/docs/installation/macos/
