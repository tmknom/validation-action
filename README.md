# validation-action

Validates whether input values meet specified rules.

<!-- actdocs start -->

## Description

Validates the input values against formats, patterns, ranges, character sets, and other user-defined rules.

## Inputs

| Name | Description | Default | Required |
| :--- | :---------- | :------ | :------: |
| value | The value to validate. | n/a | yes |
| alpha | Validates whether the input value contains only English letters (a-zA-Z). | n/a | no |
| alphanumeric | Validates whether the input value contains only English letters and digits (a-zA-Z0-9). | n/a | no |
| ascii | Validates whether the input value contains only ASCII characters. | n/a | no |
| base64 | Validates whether the input value is valid Base64. | n/a | no |
| digit | Validates whether the input value contains only digits (0-9). | n/a | no |
| domain | Validates whether the input value is a valid domain. | n/a | no |
| email | Validates whether the input value is a valid email address. | n/a | no |
| enum | Validates whether the input value matches any of the specified enumerations. | n/a | no |
| exact-length | Validates whether the input value's length is exactly equal to the specified number. | n/a | no |
| float | Validates whether the input value is a floating-point number. | n/a | no |
| int | Validates whether the input value is an integer. | n/a | no |
| json | Validates whether the input value is valid JSON. | n/a | no |
| lower-case | Validates whether the input value contains only lower case unicode letters. | n/a | no |
| max | Validates whether the input value is less than or equal to the specified maximum. | n/a | no |
| max-length | Validates whether the input value's length is less than or equal to the specified maximum. | n/a | no |
| min | Validates whether the input value is greater than or equal to the specified minimum. | n/a | no |
| min-length | Validates whether the input value's length is greater than or equal to the specified minimum. | n/a | no |
| not-empty | Validates whether the input value is not empty. | n/a | no |
| pattern | Validates whether the input value matches the specified regular expression. | n/a | no |
| printable-ascii | Validates whether the input value contains only printable ASCII characters. | n/a | no |
| semver | Validates whether the input value is a valid semantic version. | n/a | no |
| timestamp | Validates whether the input value matches a timestamp format [rfc3339, datetime, date, time]. | n/a | no |
| url | Validates whether the input value is a valid URL. | n/a | no |
| uuid | Validates whether the input value is a valid UUID. | n/a | no |

## Outputs

N/A

<!-- actdocs end -->

## Permissions

N/A

## Usage

```yaml
  steps:
    - name: Validation
      uses: tmknom/validation-action@v0
      with:
        value: foo
        not-empty: true
```

## FAQ

### What Happens When a Validation Error Occurs?

The workflow stops with an error because the action returns a non-zero exit code when it detects a validation error.

### Can I Continue Processing Despite Validation Errors?

Yes, you can use the `continue-on-error` key.
For example, you can configure the action like this to keep processing even if a validation error happens.

```yaml
- uses: tmknom/validation-action@v0
  with:
    value: foo
    not-empty: true
  continue-on-error: true
```

## Related projects

- [valid](https://github.com/tmknom/valid): The CLI tool validates whether input values meet specified rules.

## Release notes

See [GitHub Releases][releases].

[releases]: https://github.com/tmknom/validation-action/releases
