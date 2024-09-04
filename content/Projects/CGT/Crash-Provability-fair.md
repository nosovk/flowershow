## Flow
Before the round starts we show `prev` and `hash`.
After the round ends we show `salt` and `now` fields.

After user can call some verify screen, where Calculation could be done in browser.
## Calculation
`sha256(salt + prev + now) = hash`

| salt | prev | now | hash                                                             |
| ---- | ---- | --- | ---------------------------------------------------------------- |
| xxx  | 0    | 1.5 | b2517136ccefa1ab5b5dd226ff94bb4653a45f9dc3bcdd6b3ac7182c151ae854 |
| yyy  | 1.5  | 3   | d4fdbf4bcaa499e443325af3b548d71309ec9aeea9fc30a388b4592907537b3a |
| zzz  | 3    | 1   | 75c0e91450e0b38c3f5d1fafa2eb2f7b6447b0c0a01d2ad605be3f22000d8703 |

Where
- salt: just random number
- prev: previse crash point
- now: current crash point

Where hash in a first line is: `sha256(xxx01.5)=b2517136ccefa1ab5b5dd226ff94bb4653a45f9dc3bcdd6b3ac7182c151ae854`

