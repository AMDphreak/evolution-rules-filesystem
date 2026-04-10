# CI Compatibility Patterns

GitHub automatically generates source code archives for every release and tag. These archives follow a specific structure that differs from a simple "zip of internal folders".

## GitHub Auto-Archive Structure

When you download an auto-generated archive (e.g., `equivalence-rules-filesystem-1.0.0.zip`), it contains:
1. A root directory named `<repo-name>-<tag-name>/` (e.g., `equivalence-rules-filesystem-1.0.0/`).
2. The rules folder is located at `<repo-name>-<tag-name>/rules/`.

## Using the `latest` tag

To always use the most recent rules without updating your CI configuration:

1. **Download URL**: `https://github.com/AMDphreak/equivalence-rules-filesystem/archive/refs/tags/latest.zip`
2. **Internal Folder**: The root folder in this zip will be `equivalence-rules-filesystem-latest/`.

## Making Other CI Environments Compatible

If you are using a CI environment like GitLab or a custom build server that expects a flat structure or a specific zip naming convention, you can use the following logic:

### Pattern: Extract and Flatten

If your tool expects `rules/` at the root, you can extract and move:

```bash
# Example for a CI script
unzip equivalence-rules-filesystem-latest.zip
mv equivalence-rules-filesystem-latest/rules ./rules
rm -rf equivalence-rules-filesystem-latest
```

### Pattern: Directory Masking

Many extraction tools allow stripping the first component of the path:

```bash
# Using tar to strip the root directory
tar -xzf equivalence-rules-filesystem-latest.tar.gz --strip-components=1
```

## Why use Auto-Archives?

Relying on GitHub's built-in archives ensures that the release process is lightweight and less prone to workflow failures, while maintaining a consistent "Snapshot of the Repo" approach.
