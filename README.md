# bluespec.sty

Bluespec SystemVerilog (BSV) support for the LaTeX
[listings](https://www.ctan.org/pkg/listings) package.

`listings` ships a `Verilog` definition, but its vocabulary is Verilog-2001 and
shares little with BSV: no `rule`, `method`, `provisos` or `deriving`, nothing
from the standard library, and no handling of `(* ... *)` attributes. This
package defines `BSV` as a language of its own, with keyword lists taken from
bsc 2026.01, and a listings style.

## Requirements

LaTeX2e, plus `listings` and `xcolor`, which the package loads itself.

## Installation

Drop `bluespec.sty` beside your document, or install it for every document:

```sh
mkdir -p "$(kpsewhich -var-value TEXMFHOME)/tex/latex/bluespec"
cp bluespec.sty "$(kpsewhich -var-value TEXMFHOME)/tex/latex/bluespec/"
```

## Usage

```latex
\usepackage{bluespec}

\lstset{style=bsv, basicstyle=\ttfamily\small}
\lstinputlisting{Gcd.bsv}
```

`style=bsv` sets the language and colors, and nothing else: no `basicstyle`, no
column model, no frame. It composes with the listings setup the document
already has, in either order. That includes the font: listings leaves
`basicstyle` empty by default, which means the surrounding document font, so
set `\ttfamily` yourself or your code comes out in the body serif.

For the language without the colors, ask for it directly. A single
`keywordstyle` covers all five classes, since listings falls back to it for
classes that have no style of their own:

```latex
\lstset{language=BSV, basicstyle=\ttfamily\small, keywordstyle=\bfseries}
```

The `default` option applies `style=bsv` to every listing in the document, and
still leaves the typography to you:

```latex
\usepackage[default]{bluespec}
\lstset{basicstyle=\ttfamily\small}
```

Before v2.0 the language was called `{[Bluespec]Verilog}`. That name is now an
alias for `BSV` and still works, so existing documents need no edits.

## Verify

```sh
make -C example
```

builds `example/demo.pdf` from `example/Gcd.bsv`: a whole file, an inline
listing, a few `\lstinline` fragments, and a last section that takes
`language=BSV` without the style, to show that the package paints nothing
until it is asked to.

The build from `main` is published at
<https://megabyde.github.io/bsv-listings/>, so you can see the highlighting
before installing anything.

## What gets highlighted

| Class | Contents | Color |
| ----- | -------- | ----- |
| `[1]` | reserved keywords, and the contextual keywords of `import "BVI"` bodies, clock and reset declarations and typeclass dependencies | `bsvkeyword` |
| `[2]` | standard library types, typeclasses and package names | `bsvtype` |
| `[3]` | standard library functions, module constructors and interface methods | `bsvfunction` |
| `[4]` | system tasks and functions | `bsvsystemtask` |
| `[5]` | compiler directives | `bsvdirective` |

Attributes are highlighted as a whole, `(*` and `*)` included, so attribute
names need no keyword list of their own. The delimiter belongs to the style,
not to the language, so `language=BSV` on its own paints nothing at all.

SystemVerilog keywords that BSV reserves but never uses are deliberately
absent: highlighting `always` or `wire` in BSV code would suggest they mean
something.

## Customizing

The eight colors are ordinary `xcolor` names, declared with `\providecolor`.
Define one before loading the package and yours survives; redefine it after and
it takes effect from that point:

```latex
\definecolor{bsvtype}{HTML}{2A6099}
\colorlet{bsvsystemtask}{bsvfunction}
```

`bsvdirective` and `bsvattribute` start out the same gold, on the grounds that
directives and attributes both address the toolchain rather than the hardware.
They are separate names, so they can be split.

Attribute highlighting runs through one command, so it can be changed or
dropped without touching the language:

```latex
\renewcommand\bsvattributestyle{\itshape}
\renewcommand\bsvattributestyle{}        % plain attributes
```

## Caveats

The style asks for `\bfseries` on class `[1]`, and Computer Modern has no bold
typewriter shape, so LaTeX substitutes the medium weight and the request is
silently dropped. `\usepackage{lmodern}`, or any other font with a bold `tt`,
brings it back; stock `Verilog` and `C` listings lose their bold the same way.
Color carries the distinction regardless.

Sized literals such as `8'hFF` come out with a curly quote under T1 font
encoding. Load [`upquote`](https://ctan.org/pkg/upquote), as `example/demo.tex`
does, or set `upquote=true`. Under OT1 both raise
`\textquotedbl unavailable in encoding OT1`, which is why the package leaves
the choice to the document.

Class `[3]` carries library names general enough to collide with your own
identifiers: `map`, `head`, `start`, `clear`, `first`. Drop the ones that get in
the way:

```latex
\lstset{deletekeywords=[3]{start, first, clear}}
```

## Keeping up with bsc

The lists are maintained by hand, and they move slowly: between bsc 2026.01 and
the following months of `main`, the keywords and system tasks did not change at
all and the library classes gained eleven names. A release or two a year is a
handful of additions.

These are the sources in the [bsc](https://github.com/B-Lang-org/bsc) tree that
decide what belongs where:

| Class | Source |
| ----- | ------ |
| `[1]` | `src/comp/SystemVerilogKeywords.lhs`, where BSV-specific keywords carry the `Bluespec38` tag, and `src/comp/Parser/BSV/*.lhs` for the contextual ones |
| `[2]`, `[3]` | `src/Libraries`, restricted to names the Libraries Reference Guide indexes in `doc/libraries_ref_guide`; the index is what separates the public API from internal helpers |
| `[4]` | the `\index{\$...}` entries of `doc/BSV_ref_guide/BSV_lang.tex` |
| `[5]` | `src/comp/SystemVerilogPreprocess.lhs`, restricted to the directives the preprocessor acts on rather than the Verilog ones it merely tolerates |

Two rules keep the classes honest. A SystemVerilog keyword that BSV reserves
but never uses does not belong in `[1]`, and a library name common enough to
collide with ordinary user identifiers (`add`, `count`, `value`) does not
belong in `[3]`.

## License

MIT; see `LICENSE.txt`.
