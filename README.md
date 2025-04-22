This is a Hugo prototype showing how to page through all of the regular pages across multiple sections, sorting them by section weight first, then page weight.

---

Given this content structure:

```
content/
├── category-1/
│   ├── _index.md
│   ├── page-1.md
│   ├── page-2.md
│   └── page-3.md
├── category-2/
└── category-3/
```

Get all regular pages, sorting them by section weight first, then by page weight:

```go-template
{{ $pages := sort site.RegularPages.ByWeight "Parent.Weight" }}
```

---

Given this content structure:

```
content/
├── about/
├── category-1/
│   ├── _index.md
│   ├── page-1.md
│   ├── page-2.md
│   └── page-3.md
├── category-2/
└── category-3/
```

Get all regular pages in arbitrary sections:

```go-template
{{ $sections := slice "category-1" "category-2" "category-3" }}
{{ $pages := sort (where site.RegularPages.ByWeight "Section" "in" $sections) "Parent.Weight" }}
```

---

Given this content structure:

```
content/
├── about/
└── portfolio/
    ├── _index.md
    ├── category-1/
    │   ├── _index.md
    │   ├── page-1.md
    │   ├── page-2.md
    │   └── page-3.md
    ├── category-2/
    └── category-3/
```

Get all regular pages in sections under the current top-level section:

```go-template
{{ $pages := sort .FirstSection.RegularPagesRecursive.ByWeight "Parent.Weight" }}
```

---

After you have a collection of pages, you can display the pager buttons in the single page template using the `.Next` and `.Prev` methods:

```go-template
{{ with $pages.Next . }}<a href="{{ .RelPermalink }}">← {{ .Title }}</a>{{ end }}
{{ with $pages.Prev . }}<a href="{{ .RelPermalink }}">{{ .Title }} →</a>{{ end }}
```
