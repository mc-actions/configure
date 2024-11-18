# MinIO CLI Configure Action

Downloads binary and configures it for using with your favourite s3 server.

After configuring you can use [`mc` command line tool](https://min.io/docs/minio/linux/reference/minio-mc.html) or other [actions from `mc-actions`](https://github.com/mc-actions).

List of available commands you can find in [official `mc` command line tool documentation](https://min.io/docs/minio/linux/reference/minio-mc.html)


## Examples

Simply configure `mc` to use with MinIO server on `https://minio.company`. Use [GitHub secrets](https://docs.github.com/actions/security-for-github-actions/security-guides/using-secrets-in-github-actions) to provide Access and Secret keys for job.

```yaml
steps:
- uses: mc-actions/configure@v1
  with:
    s3_server: 'https://minio.company'
    s3_access_key: '${{ secrets.S3_ACCESS_KEY }}'
    s3_secret_key: '${{ secrets.S3_SECRET_KEY }}'
- shell: bash
  run: |
    mc ls default  # This line will return list of buckets in https://minio.company
```

### Google Storage configuration

Create credentials with [google manual](https://cloud.google.com/storage/docs/authentication). Save Access and Secret keys to GitHub secrets `S3_ACCESS_KEY` and `S3_SECRET_KEY` respectively.

```yaml
steps:
- uses: mc-actions/configure@v1
  with:
    s3_server: 'https://storage.googleapis.com'
    s3_access_key: '${{ secrets.S3_ACCESS_KEY }}'
    s3_secret_key: '${{ secrets.S3_SECRET_KEY }}'
    s3_api: 'S3v2'
    path: 'dns'
- shell: bash
  run: |
    mc ls default  # This line will return list of buckets in Google Storage
```

### AWS S3 configuration

Get credentials for AWS S3
- Open the [IAM console](https://console.aws.amazon.com/iam/home?#home).
- From the navigation menu, click Users.
- Select your IAM user name.
- Click User Actions, and then click Manage Access Keys.
- Click Create Access Key.
- Setup `Access key ID` and `Secret access key` to GitHub secrets `S3_ACCESS_KEY` and `S3_SECRET_KEY` respectively.


```yaml
steps:
- uses: mc-actions/configure@v1
  with:
    s3_server: 'https://s3.amazonaws.com'
    s3_access_key: '${{ secrets.S3_ACCESS_KEY }}'
    s3_secret_key: '${{ secrets.S3_SECRET_KEY }}'
    path: 'dns'
- shell: bash
  run: |
    mc ls default  # This line will return list of buckets in AWS S3
```

### Yandex Object Storage configuration

Get keys according to [this manual](https://yandex.cloud/en/docs/iam/concepts/authorization/access-key) and set `Key ID` and `Secret key` to GitHub secrets `S3_ACCESS_KEY` and `S3_SECRET_KEY` respectively.

```yaml
steps:
- uses: mc-actions/configure@v1
  with:
    s3_server: 'https://storage.yandexcloud.net'
    s3_access_key: '${{ secrets.S3_ACCESS_KEY }}'
    s3_secret_key: '${{ secrets.S3_SECRET_KEY }}'
- shell: bash
  run: |
    mc ls default  # This line will return list of buckets in Yandex Object Storage
```


## Action inputs

The following action inputs are available to configure `mc`:

| Name            | description                                                                                                                                                                                                | Example                                    |
| --------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------ |
| `server_alias`  | Any string <br/> Optional, default `default` <br/><br/> Name of server to use in CLI. Do not edit if you will use other [mc-actions](https://github.com/mc-actions).                                            | `play`                                     |
| `s3_server`     | `scheme://host[:port]/` string <br/> Required <br/><br/> Scheme and hostname of s3 server                                                                                                                       | `https://play.min.io`                      |
| `s3_access_key` | Any string <br/> Required <br/><br/> Access key, Access key ID, Key ID, etc. [Use GitHub Secrets](https://docs.github.com/actions/security-for-github-actions/security-guides/using-secrets-in-github-actions)! | `Q3AM3UQ867SPQQA43P2F`                     |
| `s3_secret_key` | Any string <br/> Required <br/><br/> Secret access key, Secret key, etc. [Use GitHub Secrets](https://docs.github.com/actions/security-for-github-actions/security-guides/using-secrets-in-github-actions)!     | `zuf+tfteSlswRu7BJ86wekitnifILbZam1KYY3TG` |
| `s3_api`        | `s3v4`, `s3v2` <br/> Optional, default `S3v4`<br/><br/> S3 API version to use                                                                                                                                   | `S3v4`                                     |
| `path`          | `auto`, `on`, `off`, `dns` <br/> Optional, default `auto`<br/><br/> Bucket path lookup supported by the server                                                                                                  | `auto`                                     |
