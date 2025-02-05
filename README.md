# validation-action

Validates whether input values meet specified rules.

<!-- actdocs start -->

## Description

Validates input values against character sets, ranges, formats, and custom rules to prevent workflow failures.

## Inputs

| Name | Description | Default | Required |
| :--- | :---------- | :------ | :------: |
| value | The value to validate against the specified rules. | n/a | yes |
| alpha | Validates that the `value` contains only English letters (a-zA-Z). | n/a | no |
| alphanumeric | Validates that the `value` contains only English letters and digits (a-zA-Z0-9). | n/a | no |
| ascii | Validates that the `value` contains only ASCII characters. | n/a | no |
| base64 | Validates that the `value` is a valid Base64 string. | n/a | no |
| digit | Validates that the `value` contains only digits (0-9). | n/a | no |
| domain | Validates that the `value` is a valid domain. | n/a | no |
| email | Validates that the `value` is a valid email address. | n/a | no |
| enum | Validates that the `value` matches one of the specified enumerations (comma-separated list). | n/a | no |
| exact-length | Validates that the length of `value` is exactly the specified number. | n/a | no |
| float | Validates that the `value` is a floating-point number. | n/a | no |
| int | Validates that the `value` is an integer. | n/a | no |
| json | Validates that the `value` is a valid JSON string. | n/a | no |
| lower-case | Validates that the `value` contains only lower case Unicode letters. | n/a | no |
| mask-value | Masks the `value` in error messages to protect sensitive data. | n/a | no |
| max | Validates that the `value` is less than or equal to the specified maximum. | n/a | no |
| max-length | Validates that the length of `value` is less than or equal to the specified maximum. | n/a | no |
| min | Validates that the `value` is greater than or equal to the specified minimum. | n/a | no |
| min-length | Validates that the length of `value` is greater than or equal to the specified minimum. | n/a | no |
| not-empty | Validates that the `value` is not empty. | n/a | no |
| pattern | Validates that the `value` matches the specified regular expression. | n/a | no |
| printable-ascii | Validates that the `value` contains only printable ASCII characters. | n/a | no |
| semver | Validates that the `value` is a valid semantic version. | n/a | no |
| timestamp | Validates that the `value` matches the timestamp format specified in the `timestamp` input (rfc3339, datetime, date, or time). | n/a | no |
| upper-case | Validates that the `value` contains only upper case Unicode letters. | n/a | no |
| url | Validates that the `value` is a valid URL. | n/a | no |
| uuid | Validates that the `value` is a valid UUID. | n/a | no |
| value-name | The name of the `value` to include in error messages. | n/a | no |

## Outputs

| Name | Description |
| :--- | :---------- |
| error-message | Output error messages when validation fails. |

<!-- actdocs end -->

## Permissions

This action does not require any additional `GITHUB_TOKEN` permissions.

## Why

This action validates input values against specified criteria, providing the following benefits:

- **Fewer errors**: Catches invalid inputs early, reducing workflow failures.
- **Time savings**: Eliminates the need for custom validation scripts.
- **Reliable**: Applies consistent rules as workflows become more complex, improving long-term maintainability.
- **Reusable**: Offers a standardized validation approach, useful in Composite Actions and Reusable Workflows.
- **Secure**: Uses dependencies pinned to digests and verified with keyless signing.

This action helps maintain data consistency across various use cases to ensure valid inputs and prevent workflow failures.

## Usage

The `value` input is required.
If omitted, the action fails immediately.
This action supports various validation rules, including:

- **Character sets**: Restricts input to specific character sets, such as ASCII, digits, or alphanumeric characters.
- **Ranges**: Ensures values fall within specified numerical, length, or date ranges.
- **Formats**: Checks if input matches formats, such as timestamps, URLs, or semantic versions.
- **Custom rules**: Matches user-defined rules like enumerations or regular expressions.

### Applying Multiple Validation Rules

```yaml
  steps:
    - name: Validation
      uses: tmknom/validation-action@v0
      with:
        value: example
        min-length: 10
        digit: true
```

When multiple validation rules are specified, **all** rules are evaluated sequentially.
Validation continues even if a rule fails.
If the input fails any rule, the error message includes all validation failures:

```shell
Validation error: The specified value "example" is invalid. Issues: the length must be no less than 10, must contain digits only.
```

### Using `enum` for Allowed Values

```yaml
  steps:
    - name: Validation
      uses: tmknom/validation-action@v0
      with:
        value: example # invalid value
        enum: admin,user,guest
```

### Using `pattern` for Custom Regex Matching

```yaml
  steps:
    - name: Validation
      uses: tmknom/validation-action@v0
      with:
        value: invalid+example.com # invalid value
        pattern: "^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$"
```

## Customizing Error Messages

Error messages can be customized by replacing the value name or masking input values.

### Replacing Value Name

This feature is helpful when running the action multiple times within a single workflow.
It clarifies which value caused the issue:

```yaml
  steps:
    - name: Validation
      uses: tmknom/validation-action@v0
      with:
        value: example
        value-name: account-id
        digit: true
```

When `value-name` is specified, the error messages replace "value" with the provided name:

```shell
Validation error: The specified account-id "example" is invalid. Issues: must contain digits only.
```

### Masking Input Value

This feature is useful for protecting sensitive data, such as tokens, passwords, or API keys, to prevent exposure in logs:

```yaml
  steps:
    - name: Validation
      uses: tmknom/validation-action@v0
      with:
        value: ${{ secrets.EXAMPLE_TOKEN }}
        mask-value: true
        alpha: true
```

When `mask-value` is `true`, the `value` input is replaced with `***` in error messages:

```shell
Validation error: The specified value "***" is invalid. Issues: must contain English letters only.
```

## FAQ

### What happens if validation errors?

The workflow fails with a non-zero exit code.
Errors appear as **error annotations**, following this format:

```shell
Validation error: The specified value "<input-value>" is invalid. Issues: <list-of-issues>
```

This message clearly indicates which rules were violated.

> [!NOTE]
>
> The error annotations are a feature of GitHub Actions called workflow commands, not this action itself.
> For more information, see the [GitHub Documentation](https://docs.github.com/en/actions/writing-workflows/choosing-what-your-workflow-does/workflow-commands-for-github-actions).

### What happens if I specify multiple validation rules?

All rules are checked sequentially.
The `value` input must satisfy **all** rules to be valid.
For example, if you specify both `digit` and `min-length`, the input must consist only of digits and be at least the specified length.

The order in which validation rules are applied is not defined.
If multiple validation rules fail, all failures will be reported at once, but their order is not defined.

### Can I hide sensitive values in error messages?

Yes, enable the `mask-value` to obscure the input as `***`.

Standard error message:

```shell
Validation error: The specified value "example" is invalid. Issues: must contain English letters only.
```

Customized error message:

```shell
Validation error: The specified value "***" is invalid. Issues: must contain English letters only.
```

### Can I customize error messages to make debugging easier?

Yes, set the `value-name` input to specify a custom name for the validated value.

Standard error message:

```shell
Validation error: The specified value "example" is invalid. Issues: must contain digits only.
```

Customized error message:

```shell
Validation error: The specified account-id "example" is invalid. Issues: must contain digits only.
```

This replaces "value" in error messages, making it easier to identify the source of errors.

### Can I fully customize the error message?

No, at this time, you cannot directly specify a custom error message.
However, you can modify parts of the message by using the `value-name` or masking sensitive values with the `mask-value`.

### Can I continue processing despite validation errors?

Yes, set `continue-on-error: true` to allow the workflow to proceed even if validation fails:

```yaml
- uses: tmknom/validation-action@v0
  with:
    value: invalid
    digit: true
  continue-on-error: true
```

This logs validation errors but allows the workflow to continue.

> [!NOTE]
>
> The `continue-on-error` setting is a feature of GitHub Actions, not this action itself.
> For more information, see the [GitHub Documentation](https://docs.github.com/en/actions/writing-workflows/workflow-syntax-for-github-actions#jobsjob_idcontinue-on-error).

### What happens if no validation rules are specified?

If no validation rules are set, the action completes successfully without validation.
No errors or warnings are generated.

This behavior is **intentional** as of now but may change in the future.
Users should be aware of this to avoid unintentionally skipping validation.

## Related projects

- [valid](https://github.com/tmknom/valid): A CLI tool that validates input values based on specified rules.
    - This action internally uses `valid` to perform input validation.
    - You can use `valid` independently for validation both locally and in CI environments.

## Release notes

See [GitHub Releases][releases].

[releases]: https://github.com/tmknom/validation-action/releases
