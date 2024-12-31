[![Jane Ori - PropJockey.io](https://img.shields.io/badge/Jane%20Ori%20%F0%9F%91%BD-%F0%9F%A4%8D%20PropJockey.io-7300E6.svg?labelColor=FB04C2&style=plastic)](http://jane.propjockey.io/)

# css-counter-value from <img src="https://github.com/user-attachments/assets/87119fb5-c39d-429a-9bfd-424f0e100720" alt="" width="30px"> PropJockey
Convert CSS `counter()` values to `integers` *without JavaScript*.

The limiting feature support required to use this utility is [`timeline-scope`](https://caniuse.com/mdn-css_properties_timeline-scope).

## `css-counter-value` Features:
1) Converting any `counter()` value into an `integer` for use in `calc()`.
2) Passing integer value of a counter UP to parents (Giving parents their child count as a --var).
3) The integer for a counter value can be set from any `counter()` contextually and be scoped all the way up on `:root` so the whole site's CSS can mathematically respond to any counter anywhere.
4) Counter values `0` to `120` supported.
5) Since counters can count globally unlike `:nth-*` selectors, elements can now mathematically respond to their GLOBAL index (<= 120).
6) Style queries then empower decendants of the `counter value`'s scope to respond in any way you can think of, beyond mathematical manipulation.
7) Need the `counter value`'s scope itself to respond in non-mathematical ways? Run a [mathematical comparitor](https://codepen.io/propjockey/pen/YzZMNaz) then convert the resulting `bit` into a [Space Toggle](https://github.com/propjockey/css-sweeper?tab=readme-ov-file#css-is-a-programming-language-thanks-to-the-space-toggle-trick) using an animation like [shown in the demo window here](https://codepen.io/propjockey/pen/dyVMgBg).
8) No JavaScript involved.

## Installation and Setup

`$ npm install css-counter-value`

Then include `/node_modules/css-counter-value/counter-value.css`

#### OR Use your favorite NPM CDN for small projects

From html:

```html
<link rel="stylesheet" type="text/css" href="https://unpkg.com/css-counter-value@2/counter-value.css">
```

#### or directly from your CSS:

```css
@import url(https://unpkg.com/css-counter-value@2/counter-value.css);
```

## Usage

Any element on the page can have a maximum of `8` converted counters `host`ed in its `scope`, each counter occupying a variable `1` through `8`.

`var(--counter-value\\1)`
`var(--counter-value\\2)`
`var(--counter-value\\3)`
`var(--counter-value\\4)`
`var(--counter-value\\5)`
`var(--counter-value\\6)`
`var(--counter-value\\7)`
`var(--counter-value\\8)`

For example, a `td` might `host` its `table-row counter` (1) and its `table-col counter` (2).

Additionally, the `table` might `host` the value of the counters of the last `td`, effectively putting two more counters in `scope` for the rest of the `table`.

This rapidly enables the table (or grid, lists, etc) to know how many rows and columns it has without writing complex selectors to account for every potential count.

In this scenario, there would still `4` additional `counters-value`s that could be converted in `scope` of any `td` on the `table`.

### `css-counter-value` Example

If you prefer to dive into live code, [check out this codepen](https://codepen.io/propjockey/pen/GgKMOdz/82aac5c9c3a29a8a23c27856b176c75d?editors=1100).

Given this table, or a similarly structured grid, nested lists, or any other DOM tree (noting the inline styles managing the counter states):

```html
<table>
  <thead>...</thead>
  <tbody style="counter-reset: table-row 0;">
    <tr style="counter-increment: table-row 1; counter-reset: table-col 0;">
      <td style="counter-increment: table-col 1;">
        Who am I, really?
      </td>
      <td style="counter-increment: table-col 1;">
        Who am I, really?
      </td>
    </tr>
    <tr style="counter-increment: table-row 1; counter-reset: table-col 0;">
      <td style="counter-increment: table-col 1;">
        Who am I, really?
      </td>
      <td style="counter-increment: table-col 1;">
        Who am I, really?
      </td>
    </tr>
  </tbody>
</table>
```

To convert a counter into an integer, add a `directive` as a descendant of the elment being counted and `css-counter-value`'s built in CSS `interpreter` will assign the output `counter-value\\*` vars as directed.

By default, without a matching `host` in a `directive`'s context, the `directive` will assign the converted `counter-value` to var `1` (of `8`) on its `immediate parent`, making the parent element an *implicit host* for the variable its setting:

```html
<table>
  <thead>...</thead>
  <tbody style="counter-reset: table-row 0;">
    <tr style="counter-increment: table-row 1; counter-reset: table-col 0;">
      <td style="counter-increment: table-col 1;">
        Who am I, really?
      </td>
      <td style="counter-increment: table-col 1;">
        <output data-counter-value style="--counter: table-col;"></output>
        Know Thyself...?
      </td>
    </tr>
    <tr style="counter-increment: table-row 1; counter-reset: table-col 0;">
      <td style="counter-increment: table-col 1;">
        Who am I, really?
      </td>
      <td style="counter-increment: table-col 1;">
        <output data-counter-value style="--counter: table-col;"></output>
        Know Thyself...?
      </td>
    </tr>
  </tbody>
</table>
```

The two `td` elements with the `data-counter-value` `directive` inside of them now hold a CSS var `var(--counter-value\\1)` with an integer value of `2`.

We set the `--counter` variable to `[the name of the counter]` we wish to convert using the `style` attribute inline, but this can be done from your CSS if you prefer.

To add the row number, we must choose a variable slot not already in `scope` (other than `1` in this case) and add another `directive` to the `td`'s. To do this, assign the desired number `1` to `8` to the `data-counter-value` attribute and set `--counter` to the `table-row` counter like so:

```html
  <td style="counter-increment: table-col 1;">
    <output data-counter-value style="--counter: table-col;"></output>
    <output data-counter-value="2" style="--counter: table-row;"></output>
    Know Thyself.
  </td>
```

The two `td` elements now *implicity host* a second CSS var `var(--counter-value\\2)` with an integer value of `1` or `2` depending on which row is looking.

`data-counter-value="7"` would mean "set `var(--counter-value\\7)`'s value to [whatever counter name's value is specified]"

If we want our `table` to know how many `rows` and `cols` it contains, we add a explicit `host` `directive attribute values` to the `table` and, in the final `td`, two `directives` that set those values based on the `counter()` states in their `td` element's `scope`:

```html
<table data-counter-value="host:3 host:4">
  ...
  <tbody style="counter-reset: table-row 0;">
    ...
    <tr style="counter-increment: table-row 1; counter-reset: table-col 0;">
      ..
      <td style="counter-increment: table-col 1;">
        <output data-counter-value style="--counter: table-col;"></output>
        <output data-counter-value="2" style="--counter: table-row;"></output>
        <output data-counter-value="3" style="--counter: table-col;"></output>
        <output data-counter-value="4" style="--counter: table-row;"></output>
        Know Thyself.
      </td>
```

Assuming this is the last `td` in the `table`, the `table` and every element inside of it will have access to `var(--counter-value\\3)` (which contains and integer count of the total number of columns in the last row) and `var(--counter-value\\4)` (which contains the integer total count of rows in the table).

Any of the `8` counter-value variables can be hosted on any ancestor, even `:root`, and be set from any counter context within the `host` element.

> [!NOTE] 
> Even though the `table-row` counter itself is stored on the `tbody`, the whole `tabel` has access to its integer counter value now.

Every `td` can use both of these total count vars without adding the additional `directives` because the `--counter-value\\3` and `--counter-value\\4` vars are inherited from `table`.

> [!CAUTION]  
> Any hosting element (matching `data-counter-value*="host:"`) must not `host` the same variable as an ancestor.
> That is, for any given element's scope, it must not have more than one ancestor hosting the same counter-value variable `1` to `8`.

> [!CAUTION]
> Within any given `host` element's context (descendants), there must not be more than one `directive` trying to set the same `counter-value` variable.

> [!TIP]
> If your counters for a specific `counter-value` variable `1` to `8` are all `0` values, the most likely scenario is a violation of either of the previous two rules.

Any number of sibling elements can host all `8` `counter-value` variables for their own contexts as long as they don't have ancestors or descendants also trying to host one of the `8` variables.

For example, these three scenarios all break for the same reason:

```html
<div data-counter-value="host:1 host:2 host:3">
  <output data-counter-value="2" style="--counter: global-div-counter;"></output>
  1
  <div>
    <output data-counter-value style="--counter: global-div-counter;"></output>
    <!-- note this ^ is setting var 1, not implicitly on the parent, but explicitly to the ancestor hosting var 1 since it exists -->
    2
  </div>
  <div>
    <output data-counter-value style="--counter: global-div-counter;"></output>
    <!-- note this ^ is setting var 1 to the same host, which is a conflict -->
    3
  </div>
</div>
```

and, very similar:

```html
<div data-counter-value="host:1 host:2 host:3">
  <output data-counter-value style="--counter: global-div-counter;"></output>
  <!-- note this ^ is setting var 1 to its parent because the parent explicitly hosts var 1 -->
  1
  <div>
    <output data-counter-value style="--counter: global-div-counter;"></output>
    <!-- note this ^ is setting var 1 to the ancestor hosting var 1 since one exists -->
    2
  </div>
</div>
```

and, the tricky one:

```html
<div data-counter-value="host:2 host:3">
  <output data-counter-value style="--counter: global-div-counter;"></output>
  <!-- note this ^ is setting var 1 to its parent implicitly -->
  1
  <div>
    <output data-counter-value style="--counter: global-div-counter;"></output>
    <!-- note this ^ is going to try setting var1 implicitly to its parent but will fail because an ancestor is implicity already hosting it -->
    2
  </div>
</div>
```

The following example is completely acceptable - there's a wide range of wild, exciting behavior still possible.
We can have all sorts of fun within the established boundaries of this relationship, if you want 😉

```html
<html data-counter-value="host:8">
  ...
  <body style="counter-reset: global-div-counter: 0;">
    <style>div { counter-increment: global-div-counter 1; }</style>

    <div data-counter-value="host:3">
      <output data-counter-value="2" style="--counter: global-div-counter;"></output>
      <!-- var 2, implicitly on parent -->
      1
      <div>
        <output data-counter-value style="--counter: global-div-counter;"></output>
        <!-- var 1, implicitly on parent -->
        2
      </div>
      <div>
        <output data-counter-value style="--counter: global-div-counter;"></output>
        <!-- var 1, implicitly on parent -->
        <output data-counter-value="3" style="--counter: global-div-counter;"></output>
        <!-- var 3, explicitly on an ancestor -->
        3
      </div>
    </div>

    <div data-counter-value="host:3">
      <output data-counter-value="2" style="--counter: global-div-counter;"></output>
      <!-- var 2, implicitly on parent -->
      4
      <div>
        <output data-counter-value style="--counter: global-div-counter;"></output>
        <!-- var 1, implicitly on parent -->
        5
      </div>
      <div>
        <output data-counter-value style="--counter: global-div-counter;"></output>
        <!-- var 1, implicitly on parent -->
        <output data-counter-value="3" style="--counter: global-div-counter;"></output>
        <!-- var 3, explicitly on an ancestor -->
        6
      </div>
    </div>

    <div data-counter-value="host:3">
      <output data-counter-value="2" style="--counter: global-div-counter;"></output>
      <!-- var 2, implicitly on parent -->
      7
      <div>
        <output data-counter-value style="--counter: global-div-counter;"></output>
        <!-- var 1, implicitly on parent -->
        8
      </div>
      <div>
        <output data-counter-value style="--counter: global-div-counter;"></output>
        <!-- var 1, implicitly on parent -->
        9
      </div>
      <div>
        <output data-counter-value style="--counter: global-div-counter;"></output>
        <!-- var 1, implicitly on parent -->
        10
      </div>
      <div>
        <output data-counter-value style="--counter: global-div-counter;"></output>
        <!-- var 1, implicitly on parent -->
        <output data-counter-value="3" style="--counter: global-div-counter;"></output>
        <!-- var 3, explicitly on an ancestor -->
        <output data-counter-value="8" style="--counter: global-div-counter;"></output>
        <!-- var 8, explicitly on :root, giving the whole document access to a variable var(--counter-value\\8) === 11 -->
        11
      </div>
    </div>
```

Have fun!

<!--
  Next project (that was in dev when I needed this counter idea to work):
  Connecting a CPU hack to this directive-operator-interpretor concept and inventing a whole new html/css based programming langauge. :3
-->

## Open Contact 👽

Please do reach out if you need help with any of this, have feature requests, want to share what you've created, or wish to learn more.

| PropJockey.io | CodePen | DEV Blog | GitHub | Mastodon |
| --- | --- | --- | --- | --- |
| [![PropJockey.io](https://raw.githubusercontent.com/propjockey/propjockey-brand/main/external-social/100px/propjockey-lines.svg)](https://propjockey.io) | [![CodePen](https://raw.githubusercontent.com/propjockey/propjockey-brand/main/external-social/100px/codepen.svg)](https://codepen.io/propjockey) | [![DEV Blog](https://raw.githubusercontent.com/propjockey/propjockey-brand/main/external-social/100px/dev.svg)](https://dev.to/janeori) | [![GitHub](https://raw.githubusercontent.com/propjockey/propjockey-brand/main/external-social/100px/github.svg)](https://github.com/propjockey) | [![Mastodon](https://raw.githubusercontent.com/propjockey/propjockey-brand/main/external-social/100px/mastodon.svg)](https://front-end.social/@JaneOri) |


[🦋@JaneOri.PropJockey.io](https://bsky.app/profile/janeori.propjockey.io)

[𝕏@Jane0ri](https://x.com/jane0ri)
