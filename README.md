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

This action supports various validation rules, including:

- **Character sets**: Restricts input to specific character sets, such as ASCII, digits, or alphanumeric characters.
- **Ranges**: Falls within specified numerical, string length, or date ranges.
- **Formats**: Follows a valid format, such as timestamps, URLs, or semantic versions.
- **User-defined rules**: Matches custom rules, such as enumerations or regular expressions.

The `value` input is required. You can specify validation rules using multiple inputs. For example:

```yaml
  steps:
    - name: Validation
      uses: tmknom/validation-action@v0
      with:
        value: example
        not-empty: true
```

### Applying Multiple Validation Rules

```yaml
  steps:
    - name: Validation
      uses: tmknom/validation-action@v0
      with:
        value: example
        digit: true
        min-length: 10
```

When multiple validation rules are specified, they are applied sequentially.
If the input value fails any rule, the error message includes all validation failures:

```shell
Validation error: The specified value "example" is invalid. Issues: the length must be no less than 10, must contain digits only.
```

### Using `enum` for Allowed Values

```yaml
  steps:
    - name: Validation
      uses: tmknom/validation-action@v0
      with:
        value: example
        enum: admin,user,guest
```

The `value` must match one of the specified options: `admin`, `user`, or `guest`.
If the input does not match any of these values, an error is returned:

```shell
Validation error: The specified value "example" is invalid. Issues: must be one of [admin user guest].
```

### Using `pattern` for Custom Regex Matching

```yaml
  steps:
    - name: Validation
      uses: tmknom/validation-action@v0
      with:
        value: invalid+example.com
        pattern: "^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$"
```

The `value` must match the specified regular expression.
If it does not match, an error is returned:

```shell
Validation error: The specified value "invalid+example.com" is invalid. Issues: must be in a valid format.
```

### Masking Input Values

```yaml
  steps:
    - name: Validation
      uses: tmknom/validation-action@v0
      with:
        value: ${{ secrets.EXAMPLE_TOKEN }}
        mask-value: true
        alpha: true
```

When `mask-value` is `true`, the `value` input is replaced with "***" in error messages:

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
        value: example
        value-name: account-id
        digit: true
```

When `value-name` is specified, it customizes error messages by replacing "value" with the given name:

```shell
Validation error: The specified account-id "example" is invalid. Issues: must contain digits only.
```

This helps distinguish which value caused the error when validating multiple values.

## FAQ

### What happens if I specify multiple validation rules?

If you specify multiple rules, the action applies them sequentially.
The `value` input must satisfy **all specified rules** to be considered valid.
For example, if you specify both `digit` and `min-length`, the input must consist only of digits and be at least the specified length.

### What happens when a validation error occurs?

The workflow fails because the action returns a non-zero exit code.
The error messages are displayed using **error annotations**, following this structure:

```shell
Validation error: The specified value "<input-value>" is invalid. Issues: <list-of-issues>
```

Example:

```shell
Validation error: The specified value "invalid" is invalid. Issues: the value must contain only digits, the length must be at least 10 characters.
```

This message clearly indicates which rules were violated.
If multiple rules fail, the error message lists all validation issues.

> [!NOTE]
>
> The error annotations are a feature of GitHub Actions called workflow commands.
> For more information, see the [GitHub Documentation](https://docs.github.com/en/actions/writing-workflows/choosing-what-your-workflow-does/workflow-commands-for-github-actions).

### What are some common validation errors?

Common validation errors include:

- Invalid format (e.g., incorrect email, URL, or timestamp).
- Length constraints not met (e.g., too short or too long).
- Value does not match any allowed options in an `enum`.

Example error messages:

- **Invalid email format**:
  ```shell
  Validation error: The specified value "invalid-email" is invalid. Issues: must be a valid email address.
  ```
- **Pattern mismatch**:
  ```shell
  Validation error: The specified value "invalid+example.com" is invalid. Issues: must be in a valid format.
  ```
- **Length too short**:
  ```shell
  Validation error: The specified value "short" is invalid. Issues: length must be at least 10 characters.
  ```

These messages provide clear feedback, helping you quickly correct the input.

### Can I hide sensitive values in error messages?

Yes, set the `mask-value: true` to replace the `value` input with `***` in error messages.
This is useful for sensitive data like tokens, passwords, or API keys.

### Can I customize error messages to make debugging easier?

Yes, set the `value-name` input to a custom name for the validated value.
This replaces "value" in error messages, making it easier to identify the source of errors.
This is especially helpful when running the action multiple times in a single workflow, as it clarifies which value caused the issue.

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
> The `continue-on-error` setting is not handled by the action itself.
> It is part of GitHub Actions' workflow syntax.
> For more information, see the [GitHub Documentation](https://docs.github.com/en/actions/writing-workflows/workflow-syntax-for-github-actions#jobsjob_idcontinue-on-error).

### What happens if no validation rules are specified?

The action runs but does not perform any validation.
To prevent unintended behavior, specify at least one rule.

## Related projects

- [valid](https://github.com/tmknom/valid): The CLI tool validates input values based on specified rules. It can be used for validation both locally and in CI environments, just like this action.

## Release notes

See [GitHub Releases][releases].

[releases]: https://github.com/tmknom/validation-action/releases
