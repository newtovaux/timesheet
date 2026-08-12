# Hours worked

A single-file timesheet calculator for a Monday to Friday week. Enter a start time, lunch out, lunch back and finish for each day, and it works out the hours per day and the week total.

No build step, no dependencies, no server. One HTML file with the CSS and JavaScript inline.

## Use it

Open `hours-worked.html` in a browser. That's it.

To serve it locally instead:

```bash
python3 -m http.server 8000
# then visit http://localhost:8000/hours-worked.html
```

To host it, drop the file anywhere that serves static content, including GitHub Pages.

## Entering times

Everything is 24-hour. The parser is deliberately lenient, so all of these give 09:30:

```
930
9:30
9.30
0930
```

A bare hour works too, `9` becomes 09:00. Fields normalise to `HH:MM` when they lose focus. `24:00` is accepted as a finish time for a shift that ends at midnight.

`Enter` moves to the next field, so a full week can be keyed without touching the mouse.

Leave both lunch fields blank and the day counts start to finish. Fill in only one and the row flags it rather than guessing.

## Output

Each day shows `H:MM` with the decimal equivalent underneath, since payroll and timesheet systems tend to want one or the other. The week total appears in the footer in both formats, along with a count of days entered.

The bar on the right of each row plots the day against a shared axis, running from the week's earliest start to its latest finish, with lunch hatched out of the middle. All five rows share that axis, so a long or ragged day is obvious without reading the numbers.

## Validation

A row reports the first problem it finds and contributes nothing to the total until it's fixed:

| Condition | Message |
| --- | --- |
| A field won't parse | Use a time like 09:30. |
| Start or finish missing | Needs a start and a finish. |
| Finish at or before start | Finish has to come after start. |
| One lunch field filled | Fill in both lunch times, or neither. |
| Lunch back before lunch out | Lunch back has to come after lunch out. |
| Lunch outside the working day | Lunch falls outside the working day. |

## Known limits

A finish earlier than the start is an error, not an overnight shift. Correct for office hours, wrong if you need to log a night.

Nothing persists on refresh. Adding `localStorage` is a few lines if you want it.

Only Monday to Friday. Weekend rows would mean extending the `DAYS` array in the script and nothing else.

The layout collapses to a stacked card per day below 760px. Print styles hide the buttons and drop the shadow.

## Customising

Colours and fonts are CSS custom properties in the `:root` block, with a `prefers-color-scheme: dark` override underneath. Changing the palette means editing those values and nothing further.

The two fonts, Archivo and IBM Plex Mono, load from Google Fonts. Remove the `<link>` tags and the page falls back to system faces without breaking.

The days, field labels and placeholder times live in the `DAYS` and `FIELDS` constants at the top of the script.

## Browser support

Anything with CSS custom properties and ES6. No polyfills, no transpilation.

## Licence

MIT.
