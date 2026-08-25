# noone.dev

Source for the website, powered by Hugo

## Notes

Add Post

```bash
hugo new content/posts/my-new-post.md
```

Styling is built with [Tailwind CSS](https://tailwindcss.com/). Install dependencies once with `npm install`, then:

```bash
npm run build:css   # regenerate static/styles/main.css (do this before `hugo`)
npm run watch:css    # rebuild on change while developing, alongside `hugo server -D`
```

`static/styles/main.css` is generated — don't edit it by hand, edit `src/styles/main.css` instead.
