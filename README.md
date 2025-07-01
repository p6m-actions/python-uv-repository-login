# Python Uv Login

![Latest Release](https://img.shields.io/github/v/release/p6m-actions/python-uv-login?style=flat-square&label=Latest%20Release&color=blue)

## Description

A GitHub Action that logs in to one or more Python repositories using uv. This action allows you to configure authentication credentials for use with uv when accessing private PyPI repositories or Artifactory instances.

## Usage

Add this action to your workflow to configure uv with authentication credentials:

```yaml
- uses: p6m-actions/python-uv-login@v1
  with:
    credentials: |
      pypi=username:password
      artifactory.company.com=user:token
```

## Inputs

| Input         | Description                                                                  | Required |
| ------------- | ---------------------------------------------------------------------------- | -------- |
| `credentials` | Authentication credentials in `registry=user:password` format. One per line. | Yes      |

## Examples

### Basic Authentication with PyPI

```yaml
- name: Setup uv
  uses: astral-sh/setup-uv@v3
- name: Login to PyPI
  uses: p6m-actions/python-uv-login@v1
  with:
    credentials: |
      pypi=username:password
```

### Multiple Repositories including Artifactory

```yaml
- name: Setup uv
  uses: astral-sh/setup-uv@v3
- name: Login to Python Repositories
  uses: p6m-actions/python-uv-login@v1
  with:
    credentials: |
      pypi=username:password
      artifactory.company.com=user:token
      private-registry.example.com=api_user:api_key
```

### Using with Test PyPI

```yaml
- name: Setup uv
  uses: astral-sh/setup-uv@v3
- name: Login to Test PyPI
  uses: p6m-actions/python-uv-login@v1
  with:
    credentials: |
      testpypi=username:password
```

## Security Note

It's recommended to use GitHub Secrets for storing credentials:

```yaml
- uses: p6m-actions/python-uv-login@v1
  with:
    credentials: |
      pypi=${{ secrets.PYPI_CREDENTIALS }}
      artifactory.company.com=user:${{ secrets.ARTIFACTORY_TOKEN }}
```

## How It Works

This action configures authentication by creating a `.netrc` file in the user's home directory with the provided credentials. The `.netrc` file is automatically used by uv for authentication when accessing private repositories.

For common registry aliases:
- `pypi` maps to `upload.pypi.org`
- `testpypi` maps to `test.pypi.org`
- Custom registries use the provided name as the hostname, or extract the hostname if a full URL is provided