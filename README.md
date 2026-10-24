# umami-pantry

Binary release pantry for Haiku packages distributed through [umami](https://github.com/sikosis/umami) when their source is not hosted online.

The Pantry is a release warehouse, not a source repository. Do not commit `.hpkg` files to Git history; attach them to GitHub Releases.

## Publishing a package

Use one published release for each product version:

1. Tag the release as `v<version>-<id>`, for example `v1.0-getback`.
2. Attach the Haiku package using `<package>-<version>-<revision>-<architecture>.hpkg`.
3. Optionally attach `<asset>.sha256` beside it.
4. Publish a new release for every update. Do not replace an asset on an existing release.

Example:

```text
Release tag: v1.0-getback
getback-1.0.0-1-x86_64.hpkg
getback-1.0.0-1-x86_64.hpkg.sha256
```

The package's internal name, version and architecture must agree with its filename and Cookbook recipe. Umami verifies the downloaded package metadata with Haiku's `package list -i` before installation.

## Cookbook recipe

Metadata belongs in [umami-cookbook](https://github.com/sikosis/umami-cookbook). A Pantry-backed recipe uses the shared repository and a product-specific tag pattern:

```json
{
  "version": 1,
  "app": {
    "id": "getback",
    "name": "GetBack",
    "description": "GetBack for Haiku.",
    "provider": "github",
    "repo": "sikosis/umami-pantry",
    "release_tag_pattern": "^v[0-9].*-getback$",
    "asset_pattern": "^getback-.*-{arch}\\.hpkg$",
    "package_name": "getback",
    "channel": "stable"
  }
}
```

The tag pattern prevents umami from confusing another Pantry product's latest release with this one. Recipes without `release_tag_pattern` continue to use the latest release from their own repository.
