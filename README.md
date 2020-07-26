# Reason Project Ideas

> **Collection of project ideas for Reason which have been requested by
people in the community.**

Links to active efforts are included in this README. It's great to
collaborate with people already working on some of these ideas, or
start your own thing. Send a PR to list your effort
under the "Active Efforts:" section, so people know how to team up.

Please discuss new project ideas on [The Reason Discord
Channel](https://discordapp.com/invite/reasonml).

## High Level Web Server/Client APIs

Simple APIs for doing web related requests/servers.
- Creating / proposing common APIs that various implementations can
  implement (even without implementation).
- Creating basic implementations of web servers/clients.

**Active Efforts:**

- [morph](https://github.com/reason-native-web/morph) simple web
  server/client APIs.
- [h2](https://github.com/anmonteiro/ocaml-h2) a full `HTTP/2`
  implementation in OCaml.
- [opium](https://github.com/rgrinberg/opium) Sinatra like web toolkit for OCaml
- [Routes](https://github.com/anuragsoni/routes) Typed routes for OCaml/ReasonML web applications (both native and client side via js_of_ocaml)
- [ReWeb](https://github.com/yawaramin/re-web/) web framework for Reason/OCaml based on the httpaf stack

## The Universal Type Specification

Write types in Reason .re files, generate various kinds of serializers and RPC frameworks based on them

- Perhaps provide a plugin architecture for people to define new backends/languages/RPC interop.
- Can convert to arbitrarily between formats.
- Versioning built-in would be a great feature (take inspiration from [milk](https://github.com/jaredly/milk)'s versioning feature). Could start with Milk as a basis (but might want to remove the automatic type inference based on compiled output since the goal here is to have explicitly readable type definitions).
- Ideally you would check in the generated serializers/deserializers into your codebase.  
  
## Cross-plat Universal Typeahead in Reason and Revery:

- For reference: [Rofi](https://github.com/davatorium/rofi) is like a
 supercharged `dmenu` replacement that exists, and is written in C++
 (screenshot below).
- One option is to port Rofi to a cross platform native app using
  [Revery](https://github.com/revery-ui/revery). Line for line ports
  are a great way to avoid bikeshedding and quickly get something out there.
- Instead of porting it line for line, you could also reimagine
  the same concept from the ground up.

Example of Rofi Linux Package:
![RofiScreenshot](https://53280.de/rofi/arc-red.png)

## Cross-plat Universal `TextInput.h`:
A small C library ideally (with some bindings from Reason/OCaml to it) that will create a
native platform text input on any platform and render it wherever you tell it to within the
window. Native rendering frameworks like Revery can do its own drawing/compositing, including
the drawing of the "text input" border/background color, and then this native `TextInput.h`
could be placed on top of this region with transparent background color. This achieves totally
native text input/selection/keyboard(important for mobile). Once the text editing is done,
somehow "transfer" the ownership of rendering the entered text back to the drawing framework,
and remove the transparent text input overlayed on top of the screen.

- Idea: Take whatever WebKit(as in Safari) does (not Chromium because iOS Chrome isn't
  actually Chrome - it's webkit). Webkit runs on iOS, Mac, Linux, and Windows so it must
  have handled the input for all of these platforms.
- Downsides: This universal approach cannot perfectly integrate into custom renderers.
  You probably couldn't clip the text input while typing etc, but this seems like a pretty
  rare use case.

## Quality Bindings To libcurl

For when someone isn't interested in using some of the newer HTTP
libraries in OCaml.

**Active Efforts:**
- [`ocurl`](https://github.com/ygrek/ocurl)

## Good first project
### update-notifier
Inspiration: https://www.npmjs.com/package/update-notifier
![Inspiration](https://cdn.discordapp.com/attachments/643515198497488948/680535642928971788/unknown.png)

It is in JS, a similar functionnality for [OPAM](https://opam.ocaml.org) or [esy](https://esy.sh) written in OCaml could be a plus.


## Reason On Rails

Rails-like scaffolding generator for certain kinds of apps. To avoid
bikeshedding, you might even consider using the exact rails workflow.

## Lightweight Universal Date/Time Library

A lightweight date/time library that doesn't rely on system dependencies,
and can configure all date conversions from config files. (Either at build
time or runtime?)
- Time (just the number of milliseconds since epoch).
- TimeRange (two Time points).
- DateTime (Human description of DateTime).
- DateTimeZone (Human description of DateTime within a time zone).
- Converting between all of these forms (based on some time zone data base config files).

Date/Time experts, please contribute by pointing to excellent libraries
and provide advice.

Approaches:
- Could just port line for line, one of the best date-time libraries out there.
  - Joda-time is an good starting reference(and the inspiration for the java8 date/time api rewrite): https://www.joda.org/joda-time/
  - [Chronology](https://github.com/davedelong/Chronology) is a Swift library with a [more nuanced approach](https://github.com/davedelong/chronology/wiki/Foundation's-API) to dates.
  
**Related Effort:**
- [ptime](https://github.com/dbuenzli/ptime)
  (It is pretty bare-bones but should have the right primitives). 

## Java Backend

The [`rehp`](https://github.com/jordwalke/rehp) project makes it
easier to implement new backends for Reason/OCaml when compiling in
the native workflow.

It can pretty easily be extended for other languages, and `rehp`
includes an example of extending it for PHP(Hack flavor).


## Automatic Command Line App Generator:

- `devDependency` you can add to your native Reason project.
- Run `generate-cli-app ./path/to/Main.re` where `Main.re` contains:

```reason
let main = (
    /**
      * Doc for `thisArg` shown in the command line.
      */
    ~thisArg="default",
    /**
      * Doc for `mandatoryArgBecausNoDefault`.
      */
    ~mandatoryArgBecauseNoDefault
    /**
      * Doc for mandatory `positionalArgWithoutName`.
      */
    positionalArgWithoutName
  ) => {
  /**
   * Your implementation goes here.
   */
};

/**
 * <GENERATE_CLI_APP>
 */
```

And then it will replace the footer comment with the argument
parsing, and help documentation code that allows you to run the
generated binary as a command line app. For example once built, you
could run:

```bash
MainCli.exe --help
```
which would provide output identical to
[cmdliner](https://github.com/dbuenzli/cmdliner)'s help output. For
an example, see `refmt --help`:

![command line help](./images/refmt.png "Example Command Line Help")

It could also generate man pages in your project's releases based on this.

**Implementation:**
- `@reason-native/pastel` is a good option for terminal highlighting.
- The simplest implementation could just parse the AST and would
  require that each named argument include a type annotation and
  would only support `int`, `string`, `floats` etc.
- Another feature would be to support an "enum" (variant) defined in
  the same file to represent the different string options.
- A more advanced version might support using an approach like
  [`milk`](https://github.com/jaredly/milk) to infer the types.
  (Would require a bit of support from Milk to be usable for other
  purposes - so this would only make sense after `milk` is extended
  to support other use cases).

**Related Effort:**
- [ppx_deriving_cmdliner](https://github.com/hammerlab/ppx_deriving_cmdliner)
  (doesn't use named arguments to generate command line API, but
  would be a good reference).

## One Click Native Project Creation/Updating

**Active Efforts:**
- [`pesy`](https://github.com/esy/pesy): Project by [Manas](https://github.com/prometheansacrifice) to make new native projects in a single command. Bonus: It will keep your project/build config up to date based on your config in `package.json` by running `esy pesy` from your project root. Makes use of great automatically created cross platform CI.


## Full Native React:
**Active Efforts:**
- [Revery](https://github.com/revery-ui/revery) an Electron-style cross platform desktop app framework with full native React-style UI implementation based on [Brisk Reconciler](https://github.com/briskml/brisk-reconciler).
![revery screenshot](./images/revery.png "Revery Screenshot")
- [Revery Packager](https://github.com/revery-ui/revery-packager) One click packaging up of Revery UI applications into installable desktop apps.


## Reason Browser Console Formatter:
Print more readable structures in the browser for debugging purposes.

**Active Efforts:**
[reason-console-formatter](https://github.com/davesnx/reason-console-formatter)

## Static Site Generator

Design pages in JSX and output a static HTML website.

**Active Efforts:**
- [TyXML](https://github.com/ocsigen/tyxml) a library for building valid HTML and SVG
- [Canopy](https://github.com/Engil/Canopy) a blog engine powered by git and OCaml.
