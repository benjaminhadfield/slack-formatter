# Slack Fmt

> Note: Fmt on Slack directory is [v0.0.1](https://github.com/benjaminhadfield/slack-fmt/tree/v0.0.1), [view on Slack](https://moosesandbox.slack.com/apps/A9QSW4XSB-fmt?page=1)
or [install](https://16el4ez0sg.execute-api.eu-west-2.amazonaws.com/dev/oauth/direct-install) to your workspace

**Fmt** is a helpful Slack bot for formatting JSON messages 🙃

Slash command `/fmt [options] json`.

  - [Usage](#usage)
  - [Options](#options)
  - [Relaxed JSON Support](#relaxed-json-support)
  - [Multiple Commands](#multiple-commands)

### Usage

Simply type
```
/fmt {"people": [{"name": "Ben"}, {"name": "Sam"}, {"name": "Rebecca"}]}
```

and **Fmt** will respond with the JSON properly formatted.

```
{
  "people": [
    {
      "name": "Ben"
    },
    {
      "name": "Sam"
    },
    {
      "name": "Rebecca"
    }
  ]
}
```

### Options

| Option | Flag | Description |
| ------ | ---- | ----------- |
| [Custom indent size](#custom-indent-size) | `--indent` / `-i` | Specify the number of spaces to indent the formatted JSON with (default 2).
| [Tag](#tagged-responses) | `--tag` / `-t` | Add a tag above the formatted JSON (useful for naming when sending multiple fomratted messages).

#### Custom Indent Size

> `[--indent size] [-i size]`

By default **Fmt** will indent with two spaces. To change this, specify the number of spaces you'd like with the `--indent` flag.

```
/fmt --indent 4 {"a": ["one", "two", "three", "four"]}
```
```
{
    "a": [
        "one",
        "two",
        "three",
        "four"
    ]
}
```

#### Tagged Responses

> `[--tag tag] [-t tag]`

You can add a one word tag, which is displayed above the formatted response, with the `--tag` flag.

```
/fmt --tag Request {"data": [42, 189, 290]}
```
**Request**
```
{
  "data": [
    42,
    189,
    290
  ]
}
```

### Relaxed JSON Support

**Fmt** can also handle relaxed JSON, thanks to the [jsonic](https://github.com/rjrodger/jsonic) library.

You can find the rules for relaxed JSON [here](https://github.com/rjrodger/jsonic), but they are repeated below too.

  * You don't need to quote property names: `{ foo:"bar baz", red:255 }`
  * You don't need the top level braces: `foo:"bar baz", red:255`
  * You don't need to quote strings with spaces: `foo:bar baz, red:255`
  * You _do_ need to quote strings if they contain a comma or closing brace or square bracket: `icky:"_,}]_"`
  * You can use single quotes for strings: `Jules:'Cry "Havoc," and let slip the dogs of war!'`
  * You can have trailing commas: `foo:bar, red:255, `

For example

```
/fmt cats: [socks, spice, scratch, mister purrfect]

{
  "cats": [
    "socks",
    "spice",
    "scratch",
    "mister purrfect"
  ]
}
```

```
/fmt -i 4 cast: [{name: Belinda Blumenthal, job: Worldwide Sales Director of Steele's Pots & Pans}, {name: Ken Dewsbury, job: Central and Northern England RSM }]

{
    "cast": [
        {
            "name": "Belinda Blumenthal",
            "job": "Worldwide Sales Director of Steele's Pots & Pans"
        },
        {
            "name": "Ken Dewsbury",
            "job": "Central and Northern England RSM"
        }
    ]
}
```

### Multiple Commands

**Fmt** can handle multiple slash commands in one message,
just begin each individual command with `/fmt`.

This can be particularly useful when combined with tags, for example when
sending request/response JSON.

```
/fmt -t request name: Hiccup /fmt -t response id: 1594, name: Hiccup
```
**request**
```
{
  "name": "Hiccup"
}
```

**response**
```
{
  "id" 1594,
  "name": "Hiccup"
}
```
