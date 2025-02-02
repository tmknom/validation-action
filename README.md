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
| mask-value | Masks the `value` input in error messages to protect sensitive data. | n/a | no |
| max | Validates whether the input value is less than or equal to the specified maximum. | n/a | no |
| max-length | Validates whether the input value's length is less than or equal to the specified maximum. | n/a | no |
| min | Validates whether the input value is greater than or equal to the specified minimum. | n/a | no |
| min-length | Validates whether the input value's length is greater than or equal to the specified minimum. | n/a | no |
| not-empty | Validates whether the input value is not empty. | n/a | no |
| pattern | Validates whether the input value matches the specified regular expression. | n/a | no |
| printable-ascii | Validates whether the input value contains only printable ASCII characters. | n/a | no |
| semver | Validates whether the input value is a valid semantic version. | n/a | no |
| timestamp | Validates whether the input value matches a timestamp format [rfc3339, datetime, date, time]. | n/a | no |
| upper-case | Validates whether the input value contains only upper case unicode letters. | n/a | no |
| url | Validates whether the input value is a valid URL. | n/a | no |
| uuid | Validates whether the input value is a valid UUID. | n/a | no |
| value-name | The name of the `value` input to include in error messages. | n/a | no |

## Outputs

| Name | Description |
| :--- | :---------- |
| error-message | Output error messages when validation fails. |

<!-- actdocs end -->

## Permissions

N/A

## Why

This action validates input values against specified criteria, offering the following benefits:

- **Increased reliability**: Ensures consistent validation as workflows grow in complexity, supporting long-term maintainability.
- **Fewer errors**: Catches input-related issues early by enforcing predefined standards.
- **Time savings**: Eliminates the need for custom validation scripts, freeing up time for more critical tasks.
- **Better reusability**: Provides a standardized validation approach, particularly useful in Composite Actions and Reusable Workflows.
- **Stronger security**: Uses dependencies pinned to digests and verified with keyless signing for secure execution.

This action helps maintain data consistency across various use cases.

## Usage

### Applying Multiple Validation Rules

```yaml
  steps:
    - name: Validation
      uses: tmknom/validation-action@v0
      with:
        value: invalid
        digit: true
        min-length: 10
```

When multiple validation rules are specified, they are applied sequentially.
If the input value fails any rule, the error message includes all validation failures:

```shell
Validation error: The specified value "invalid" is invalid. Issues: the length must be no less than 10, must contain digits only.
```

### Masking Input Values

```yaml
  steps:
    - name: Validation
      uses: tmknom/validation-action@v0
      with:
        value: ${{ secrets.INVALID_TOKEN }}
        mask-value: true
        alpha: true
```

When `mask-value` is set to `true`, the `value` input is masked in error messages:

```shell
Validation error: The specified value "***" is invalid. Issues: must contain English letters only.
```

This is particularly useful for masking sensitive data like tokens or passwords to prevent exposure in logs.

### Enhancing Error Message Clarity

```yaml
  steps:
    - name: Validation
      uses: tmknom/validation-action@v0
      with:
        value: invalid
        value-name: account-id
        digit: true
```

When `value-name` is specified, it customizes error messages by replacing "value" with the given name:

```shell
Validation error: The specified account-id "invalid" is invalid. Issues: must contain digits only.
```

This helps distinguish which value caused the error when validating multiple values.

## FAQ

### What happens when a validation error occurs?

The workflow fails because the action returns a non-zero exit code.
Check the error annotation for details.

### What happens if I specify multiple validation rules?

If you specify multiple rules (e.g., `alpha`, `max-length`, `not-empty`),
the action applies all of them in sequence.
The input must satisfy all rules to be considered valid.

### Can I hide sensitive values in error messages?

Yes, setting `mask-value: true` will mask the `value` input in error messages.
This is useful for sensitive data like tokens, passwords, or API keys.

### Can I customize error messages for better clarity?

Yes, use `value-name` to provide a custom name for the validated value.
This name appears in error messages, improving clarity.

### Can I continue processing despite validation errors?

Yes, set `continue-on-error: true`.
For example, this configuration allows processing to continue even if validation fails:

```yaml
- uses: tmknom/validation-action@v0
  with:
    value: invalid
    digit: true
  continue-on-error: true
```

### What happens if no validation rules are specified?

The action runs but performs no validation.
Specify at least one rule to enable validation.

## Related projects

- [valid](https://github.com/tmknom/valid): The CLI tool validates whether input values meet specified rules.

## Release notes

See [GitHub Releases][releases].

[releases]: https://github.com/tmknom/validation-action/releases
