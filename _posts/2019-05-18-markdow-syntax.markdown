---
author: lexiao
comments: true
date: 2019-05-18 17:11:11+00:00
layout: post
link: http://localhost/blog/?p=442
published: true
slug: null
title: markdown syntax
wordpress_id: 442
categories:
- markdown
---

## Fenced code blocks inside ordered and unordered lists

1. This is a numbered list.
2. I'm going to include a fenced code block as part of this bullet:

    ```
    Code
    More Code
    ```

3. We can put fenced code blocks inside nested bullets, too.
   1. Like this:

        ```c
        printf("Hello, World!");
        ```

   2. The key is to indent your fenced block by **(4 * bullet_indent_level)** spaces.
   3. Also need to put a separating newline above and below the fenced block.

---

## Failed attempts:

1. This is a bullet.
2. This is the end of the first bullet list, with a fenced code block following:

```
Code
More Code
```

3. This is the start of a new bullet list. Hey, where is my fenced code? ```
Code
More Code
```

4. Maybe we can do it with indenting?
    Code
    More Code
5. Did that work? No, so we will try with separation:

    Code
    More Code

6. Now I will try more indenting, to match the bullet margin, plus 4:

       Code
       More Code

7.  Well that was disappointing. How about we indent and use backticks together?
       ```
       Code
       More Code
       ```
8. No such luck.

---

* Oh look, it's some [Apache config](http://pygments.org/docs/lexers/#lexers-for-non-source-code-file-types):

    ```apache
    <Directory /foo/bar>
      Order allow,deny
      Deny from all
    </Directory>
    ```

* Wasn't that fun?
