# imd
What if Markdown had interactive features that allowed it to replace HTML+JavaScript in more areas?


## imd-browser

### Summary
A simple textmode web browser inspired by links that allows to browse interactive markdown files. The files are
rendered as-is, but the links are followable and the forms fillable.

### Planned features
Navigate the links with up and down arrows and follow with Enter key.
```
[link name](http://link.com)
```

Fill and submit single-line forms.
```
First name: __________________ [Submit]
```

Fill and submit multi-line forms.
```
  - Enter your feedback -----
 |~                          |
 |~                          |
 |~                          |
  ---------------------------
                   [ Submit ]
```

Checkboxes in forms.
```
[ ] foo
[x] bar
```
