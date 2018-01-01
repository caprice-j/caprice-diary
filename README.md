
## Use this repository as a parent directory of caprice-j.github.io
```
caprice-diary/    # root dir of this GitHub repository
   - public/      # root dir of caprice-j.github.io repository
```

## Workflow to add a new article

1. Write a new post by Tools > Addins > Browse Addins > "New Post" > Execute.
2. push under caprice-diary/ and public/.

See [blogdown documentation](https://bookdown.org/yihui/blogdown/github-pages.html) for explanation on this structure.

+ public/ is excluded from sync and included in caprice-j.github.io repository
  - needs different commit/push
+ RStudio's project points to caprice-diary directory