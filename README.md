[![Jane Ori - PropJockey.io](https://img.shields.io/badge/Jane%20Ori%20%F0%9F%91%BD-%F0%9F%A4%8D%20PropJockey.io-7300E6.svg?labelColor=FB04C2&style=plastic)](http://jane.propjockey.io/)

# css-counter-value from <img src="https://github.com/user-attachments/assets/87119fb5-c39d-429a-9bfd-424f0e100720" alt="" width="30px"> PropJockey
Convert CSS `counter()` values to `integers` *without JavaScript*.

The limiting feature support required to use this utility is [`timeline-scope`](https://caniuse.com/mdn-css_properties_timeline-scope).

## `css-counter-value` Features:
1) Converting any `counter()` value into an `integer` for use in `calc()`.
2) Passing integer value of a counter UP to parents (Giving parents child count).
3) Passing integer counter value UP a maximum of five levels (from inside a `td` up to the element above its `table`)
4) Global indexing of elements with integers their style can respond to.
5) Counter values `0` to `120` supported.
6) No JavaScript involved.

## Installation and Setup

`$ npm install css-counter-value`

Then include `/node_modules/css-counter-value/counter-value.css`

#### OR Use your favorite NPM CDN for small projects

From html:

```html
<link rel="stylesheet" type="text/css" href="https://unpkg.com/css-counter-value@1/counter-value.css">
```

#### or directly from your CSS:

```css
@import url(https://unpkg.com/css-counter-value@1/counter-value.css);
```

## Usage

Any element on the page can have a maximum of `8` converted counters in its scope, each counter occupying a variable `1` through `8`.

For example, a `td` might have its `table-row counter` (1) and its `table-col counter` (2).

Additionally, the last `td` on the table might scope its `table-row counter` and `table-col counter` up to the parent table, effectively putting two more counters in scope for the rest of the table.

This enables the table (or grid) to know how many rows and columns it has without writing complex selectors to account for every potential count.

In this scenario, there are still `4` additional counters that could be converted in scope of any `td` on the `table`.

### `css-counter-value` Example

Given this table, or a similarly structured grid, nested lists, or any other DOM tree - note the inline styles managing the counter states.

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

To convert a counter into an integer, add a `directive` as a descendant of the elment being counted and `css-counter-value`'s built in CSS `interpreter` will assign the output vars as directed.

By default, the `directive` will assign the converted counter to output number `1` (of `8`) on its immediate parent.

It can assign the converted counter to an ancestor up to `5` levels above itself.

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

The two `td` elements now hold a CSS var `var(--counter-value\\1)` with an integer value of `2`.

We set the `--counter` variable to `[the name of the counter]` we wish to convert using the `style` attribute inline, but this can be done from your CSS if you prefer.

To add the row number, we must choose a variable slot other than `1`. To do this, assign the desired number `1` to `8` to the `data-counter-value` attribute like so:

```html
  <td style="counter-increment: table-col 1;">
    <output data-counter-value style="--counter: table-col;"></output>
    <output data-counter-value="2" style="--counter: table-row;"></output>
    Know Thyself.
  </td>
```

The two `td` elements now hold a CSS var `var(--counter-value\\2)` with an integer value of `1` or `2` depending on which row is looking.

If we pass up to the `table` counter values for the `row` and `col` of the last `td` in the `table`, its styles will then be able to respond to the number of columns and rows it has inside easily.

To do this, we must add `directives` with further instruction like so:

```html
<table>
  ...
  <tbody style="counter-reset: table-row 0;">
    ...
    <tr style="counter-increment: table-row 1; counter-reset: table-col 0;">
      ..
      <td style="counter-increment: table-col 1;">
        <output data-counter-value style="--counter: table-col;"></output>
        <output data-counter-value="2" style="--counter: table-row;"></output>
        <output data-counter-value="3:on-nth-parent(4)" style="--counter: table-col;"></output>
        <output data-counter-value="4:on-nth-parent(4)" style="--counter: table-row;"></output>
        Know Thyself.
      </td>
```

Assuming this is the last `td` in the `table`, the `table` and every element inside of it will have access to `var(--counter-value\\3)` which contains the total number of columns in the last row and `var(--counter-value\\4)` which contains the total number of rows in the table.

The `:on-nth-parent(X)` `operator` must be preceeded by a number `1` to `8` and contain a value in its parentheses `1` to `5`.

Note: Even though the `table-row` counter itself is stored on the `tbody`, the whole `tabel` has access to its integer counter value.

Since they're on the `table`, every `td` can use both of these vars without adding the additional `directives` because the `--counter-value\\3` and `--counter-value\\4` vars are inherited.

For any given scope, each of the `8` available counters can only be set at most once.

If a `directive` sets a variable on an element, no other `directives` inside that element's scope can re-use that same variable.

If more than one does happen in the same scope, the value will fail to `0` until it is resolved.

## Open Contact 👽

Please do reach out if you need help with any of this, have feature requests, want to share what you've created, or wish to learn more.

| PropJockey.io | CodePen | DEV Blog | GitHub | Mastodon |
| --- | --- | --- | --- | --- |
| [![PropJockey.io](https://raw.githubusercontent.com/propjockey/propjockey-brand/main/external-social/100px/propjockey-lines.svg)](https://propjockey.io) | [![CodePen](https://raw.githubusercontent.com/propjockey/propjockey-brand/main/external-social/100px/codepen.svg)](https://codepen.io/propjockey) | [![DEV Blog](https://raw.githubusercontent.com/propjockey/propjockey-brand/main/external-social/100px/dev.svg)](https://dev.to/janeori) | [![GitHub](https://raw.githubusercontent.com/propjockey/propjockey-brand/main/external-social/100px/github.svg)](https://github.com/propjockey) | [![Mastodon](https://raw.githubusercontent.com/propjockey/propjockey-brand/main/external-social/100px/mastodon.svg)](https://front-end.social/@JaneOri) |


[🦋@JaneOri.PropJockey.io](https://bsky.app/profile/janeori.propjockey.io)

[𝕏@Jane0ri](https://x.com/jane0ri)
