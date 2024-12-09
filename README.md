# validation-action

Validate specified value in workflows.

<!-- actdocs start -->

## Description

This action validates input values based on user-specified rules.
It integrates seamlessly with Composite Actions, Reusable Workflows, and standard GitHub Workflows,
making it versatile for input validation across various use cases.

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
