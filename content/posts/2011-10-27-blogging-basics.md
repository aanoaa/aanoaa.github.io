---
layout: post
title: "Blogging basics"
date: 2011-10-27 04:38
comments: true
categories: octopress
---

- [http://pages.github.com/](http://pages.github.com/)
- [http://octopress.org/docs/](http://octopress.org/docs/)
- [http://octopress.org/docs/blogging/](http://octopress.org/docs/blogging/)
- [http://octopress.org/docs/configuring/](http://octopress.org/docs/configuring/)
- [http://octopress.org/docs/deploying/](http://octopress.org/docs/deploying/)

### new post

    rake new_post["Blogging basics"]

`source/_posts/YYYY-MM-DD-blogging-basics.markdown` 파일이 맨들어짐

    ---
    layout: post
    title: "Blogging basics"
    date: 2011-10-27 04:38
    comments: true
    categories: octopress               # 여러개도 가능함   ex: [one, two, three]
    published: false                    # optional for draft
    author: [Yongbin Yu, Keedi Kim]     # optional for multiple author
    ---

`.markdown` 대신에 `.html` 로도 맨들 수 있음

### generate, preview and deploy

    rake generate
    rake watch
    rake preview    # http://localhost:4000/

[POW](http://pow.cx/) 라는 것도 이용할 수 있는가 보네 멋진뎅

    rake deploy
    git add .                       # 여기부턴 optional, 하는게 좋겟지
    git cm -am "what the commit"
    git push origin source

`source/_posts/YYYY-MM-DD-blogging-basics.markdown` 의 전체 내용

```
---
layout: post
title: "Blogging basics"
date: 2011-10-27 04:38
comments: true
categories: octopress
---

- [http://octopress.org/docs/](http://octopress.org/docs/)
- [http://octopress.org/docs/blogging/](http://octopress.org/docs/blogging/)
- [http://octopress.org/docs/configuring/](http://octopress.org/docs/configuring/)
- [http://octopress.org/docs/deploying/](http://octopress.org/docs/deploying/)

### new post

    rake new_post["Blogging basics"]

`source/_posts/YYYY-MM-DD-blogging-basics.markdown` 파일이 맨들어짐

    ---
    layout: post
    title: "Blogging basics"
    date: 2011-10-27 04:38
    comments: true
    categories: octopress               # 여러개도 가능함   ex: [one, two, three]
    published: false                    # optional for draft
    author: [Yongbin Yu, Keedi Kim]     # optional for multiple author
    ---

`.markdown` 대신에 `.html` 로도 맨들 수 있음

### generate, preview and deploy

    rake generate
    rake watch
    rake preview    # http://localhost:4000/

[POW](http://pow.cx/) 라는 것도 이용할 수 있는가 보네 멋진뎅

    rake deploy
    git add .                       # 여기부턴 optional, 하는게 좋겟지
    git cm -am "what the commit"
    git push origin source
```
