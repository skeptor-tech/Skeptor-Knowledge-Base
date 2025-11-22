# rudof — Rust RDF validation toolkit

## Tags

- semantic-web/rdf
- data-quality/validation
- rust

rudof is an open-source toolkit for working with Shape Expressions, SHACL,
DCTAP, and other RDF modeling formats. The project ships a Rust library, CLI,
and Python bindings, so you can embed the validators in services, run one-off
checks against RDF dumps or SPARQL endpoints, or translate between schema
dialects while keeping prefix maps, ASTs, and comparators in sync.

## Using the resource

- Use the landing page navigation to jump into Overview, Modules, and
  Architecture sections to understand how `rudof_cli`, `rudof_lib`, and the
  supporting crates (prefix maps, SRDF interface, converters, comparators) hang
  together before adopting it in a pipeline.
- Grab binaries from the latest GitHub release or compile from source with
  `cargo` to run commands like `rudof shex-validate`, `rudof shacl-validate`,
  `rudof convert`, or `rudof generate`, which cover validation, transformation,
  graph comparison, and code generation workflows for RDF data.
- Browse the GitHub wiki for How-to guides, FAQs, ADRs, and module documentation
  to see concrete CLI samples, Python-binding examples (`pyrudof`), and notes on
  the MCP/service tooling before wiring the library into agents or automation.
- Track related projects such as ShEx.js, SHACL-s, and Oxigraph listed on the
  page whenever you need complementary engines or cross-language references for
  semantic data quality work.

## Resources

- [rudof landing page](https://rudof-project.github.io/rudof/)
- [GitHub repository](https://github.com/rudof-project/rudof)
- [Latest releases & binaries](https://github.com/rudof-project/rudof/releases/latest)
- [How-to guides](https://github.com/rudof-project/rudof/wiki/How%E2%80%90to-guides)
- [FAQ](https://github.com/rudof-project/rudof/wiki/FAQ)
- [pyrudof documentation](https://pyrudof.readthedocs.io/en/stable/)
