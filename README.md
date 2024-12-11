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
| value | The value for validation. | n/a | yes |
| digit | Checks whether the input value contains only digits (0-9). | n/a | no |
| not-empty | Checks whether the input value is not empty. | n/a | no |

## Outputs

N/A

<!-- actdocs end -->

## Permissions

N/A

## FAQ

N/A

## Related projects

N/A

## Release notes

See [GitHub Releases][releases].

## License

Apache 2 Licensed. See [LICENSE](LICENSE) for full details.

[releases]: https://github.com/tmknom/validation-action/releases
