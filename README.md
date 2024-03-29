# build-rpm

<a href="https://github.com/Cynest/build-rpm/actions"><img alt="build-rpm status" src="https://github.com/Cynest/build-rpm/workflows/test/badge.svg"></a>

</p>

This action builds a RPM package for Centos7.

## Input options

- `spec_file` : **required** Path to your _RPM spec_ file
- `variables` : New line delimited `key=value` pairs. This get's propaged to the _rpmbuild_ command as a `--define` parameter. You can then use this as a macro inside the _spec_ file.

## Outputs

- `rpm_package_path` : Path to the built RPM file
- `rpm_package_name` : Name of the built RPM file

## Usage

Basic:

```yml
- steps:
    - uses: Cynest/build-rpm@v1
      id: build_rpm
      with:
        spec_file: my_app.spec

    - name: Upload RPM
        uses: actions/upload-release-asset@v1
        env:
          GITHUB_TOKEN: ${{ github.token }}
        with:
          upload_url: my_app.rpm
          asset_path: ${{ steps.build_rpm.outputs.rpm_package_path }}
          asset_name: ${{ steps.build_rpm.outputs.rpm_package_name }}
          asset_content_type: application/octet-stream
```

With variables:

```yml
- steps:
    - uses: Cynest/build-rpm@v1
      id: build_rpm
      with:
        spec_file: my_app.spec
        variables: |
          _version=1.0.0
          _foo=bar

    - name: Upload RPM
        uses: actions/upload-release-asset@v1
        env:
          GITHUB_TOKEN: ${{ github.token }}
        with:
          upload_url: my_app.rpm
          asset_path: ${{ steps.build_rpm.outputs.rpm_package_path }}
          asset_name: ${{ steps.build_rpm.outputs.rpm_package_name }}
          asset_content_type: application/octet-stream
```
