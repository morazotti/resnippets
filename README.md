# Resnippets.el

**Resnippets** is an Emacs package for defining auto-expanding snippets triggered by regular expressions. Unlike standard snippet engines that often rely on fixed keywords or word boundaries, Resnippets matches against the text immediately preceding the cursor, enabling powerful suffix-based and context-aware expansions.

## Features

- **Regex Triggers**: Define triggers using regular expressions matching text before the cursor.
- **Group Capture**: reuse captured groups from your regex in the expansion.
- **Conditional Expansion**: Restrict snippets to specific major modes or arbitrary predicates (e.g., only inside LaTeX fragments).
- **Cursor Placement**: Control exactly where the cursor lands after expansion.

## Installation

Add `resnippets.el` to your load path and require it.

```elisp
(add-to-list 'load-path "/path/to/resnippets")
(require 'resnippets)
(resnippets-global-mode 1)
```

## Usage

### Basic Snippets

Use `resnippets-add` to register a snippet. The first argument is the regex, and the second is the expansion list.

```elisp
;; Simple replacement: typing "hat" expands to "hat-expanded"
(resnippets-add "hat" '("hat-expanded"))

;; Regex capture: "barhat" -> "\hat{bar}"
;; 1 refers to the first capture group
(resnippets-add "\\([a-zA-Z]+\\)hat" '("\\hat{" 1 "}"))
```

### Cursor Placement

Use `(resnippets-cursor)` to specify the final cursor position.

```elisp
;; "testhat" -> "\hat{test|}"
(resnippets-add "\\([a-z]+\\)hat" '("\\hat{" 1 (resnippets-cursor) "}"))
```

### Conditional Snippets

You can restrict snippets using `:mode` or `:condition`.

```elisp
;; Only expand in org-mode
(resnippets-add "alpha" "\\alpha" :mode 'org-mode)

;; Only expand if inside a LaTeX fragment (requires org-mode predicate)
(resnippets-add "\\([a-zA-Z]+\\)hat" '("\\hat{" 1 "}")
                :mode 'org-mode
                :condition '(org-inside-LaTeX-fragment-p))
```

### Group Definitions

Use `resnippets-define` to define multiple snippets with shared properties. You can optionally label the group.

```elisp
(resnippets-define "math-mode-snippets"
 '(:mode (org-mode LaTeX-mode)
   :condition (or (texmathp) (org-inside-LaTeX-fragment-p)))
 
 ("alpha" "\\alpha")
 ("beta" "\\beta")
 ("<\\([a-zA-Z0-9_^\\\\{}]+\\)|\\([a-zA-Z0-9_^\\\\{}]+\\)>" '("\\braket{" 1 "}{" 2 "}"))
 )
```

### Management

```elisp
(resnippets-remove "regex-key") ;; Remove specific snippet
(resnippets-clear)              ;; Remove all snippets
```

## Contributing

Pull requests are welcome. For major changes, please open an issue first to discuss what you would like to change.

## License

[GPLv2](LICENSE)
