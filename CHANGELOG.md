# Changelog

## v2.0 (2026-08-23)

The package now stands on its own: `\usepackage{bluespec}` used to fail unless
the document had already loaded `listings`.

* Loads `listings` and `xcolor`, and identifies itself with `\ProvidesPackage`.
* Language renamed to `BSV`. The old `{[Bluespec]Verilog}` name is an alias and
  keeps working.
* Keyword lists rebuilt against bsc 2026.01: current BSV keywords, standard
  library types, typeclasses, functions, module constructors, interface methods
  and system tasks. The system tasks gained nineteen entries, among them
  `$error`, `$fatal`, `$info`, `$warning` and the `$swrite` and `$sformat`
  families. Dropped `do`, `extern`, `local` and `localparam`, which BSV
  reserves but never uses.
* Fixed `'` being treated as a string delimiter, which mangled every sized
  literal from `8'hFF` onwards.
* `$` is a letter now, so `$display` and `$test$plusargs` match as single
  tokens.
* Compiler directives (`` `define ``, `` `ifdef ``, `` `include `` and the rest
  the preprocessor acts on) are highlighted; backtick is a letter now.
* Attributes are highlighted as a whole, `(*` and `*)` included, through
  `\bsvattributestyle`.
* Added the `bsv` listings style, eight redefinable colors, and the `default`
  package option that applies the style document-wide. The style sets colors
  only, and the package paints nothing until it is selected: colors use
  `\providecolor`, and the attribute delimiter belongs to the style rather than
  to the language.
* Added `example/`, a demo document built in CI and published to GitHub
  Pages at <https://megabyde.github.io/bsv-listings/>.

## v1.0 (2013)

Initial `\lstdefinelanguage[Bluespec]{Verilog}` definition.
