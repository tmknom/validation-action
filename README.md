# validation-action

Validates whether input values meet specified rules.

<!-- actdocs start -->

## Description

This action validates the input values against formats, patterns, ranges, character sets, and other user-defined rules.
It supports a variety of validations, including:

- **Character validation**: Checks if input uses specific character sets like ASCII, digits, or alphanumeric characters
- **Range validation**: Verifies that values fall within specified numerical or date ranges
- **Pattern matching**: Validates formats such as email addresses, URLs, or semantic versions

This action is ideal for Composite Actions, Reusable Workflows, and standard workflows in GitHub Actions.
It ensures data consistency across various use cases.

## Usage

```yaml
  steps:
    - name: Validation
      uses: tmknom/validation-action@v0
      with:
        value: foo
        not-empty: true
```

## Inputs

| Name | Description | Default | Required |
| :--- | :---------- | :------ | :------: |
| value | The value to validate. | n/a | yes |
| alpha | Validates whether the input value contains only English letters (a-zA-Z). | n/a | no |
| alphanumeric | Validates whether the input value contains only English letters and digits (a-zA-Z0-9). | n/a | no |
| ascii | Validates whether the input value contains only ASCII characters. | n/a | no |
| digit | Validates whether the input value contains only digits (0-9). | n/a | no |
| email | Validates whether the input value is a valid email address. | n/a | no |
| enum | Validates whether the input value matches any of the specified enumerations. | n/a | no |
| exact-length | Validates whether the input value's length is exactly equal to the specified number. | n/a | no |
| float | Validates whether the input value is a floating-point number. | n/a | no |
| int | Validates whether the input value is an integer. | n/a | no |
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

## Outputs

N/A

<!-- actdocs end -->

## Permissions

N/A

## FAQ

N/A

## Related projects

- [valid](https://github.com/tmknom/valid): The CLI tool validates whether input values meet specified rules.

## Release notes

See [GitHub Releases][releases].

## License

Apache 2 Licensed. See [LICENSE](LICENSE) for full details.

[releases]: https://github.com/tmknom/validation-action/releases
