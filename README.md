# GitHub Action to delete files from S3 Bucket

Fork of [s3-rm-action](https://github.com/vitorsgomes/s3-rm-action).

This simple action uses the [vanilla AWS CLI](https://docs.aws.amazon.com/cli/index.html) to delete files from a S3 bucket and delete the bucket.

## Usage

### `workflow.yml` Example

Place in a `.yml` file such as this one in your `.github/workflows` folder. [Refer to the documentation on workflow YAML syntax here.](https://help.github.com/en/articles/workflow-syntax-for-github-actions)

All [`aws s3 rm flags`](https://docs.aws.amazon.com/cli/latest/reference/s3/rm.html) are optional to allow for maximum customizability (that's a word, I promise) and must be provided by you via `args:`.

```yaml
name: Delete branch folder

on:
  delete:
    branches:
      - '*'
      - '!master'
jobs:
  remove:
    runs-on: ubuntu-latest
    
    steps:
    - name: Remove from S3
      uses: vitorsgomes/s3-rm-action@master
      with:
        args: --recursive
      env:
        AWS_S3_BUCKET: ${{ secrets.AWS_BUCKET_NAME }}
        AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}
        AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
        AWS_REGION: ${{ secrets.AWS_REGION }}
```


### Configuration

The following settings must be passed as environment variables as shown in the example. Sensitive information, especially `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY`, should be [set as encrypted secrets](https://help.github.com/en/articles/virtual-environments-for-github-actions#creating-and-using-secrets-encrypted-variables) — otherwise, they'll be public to anyone browsing your repository's source code and CI logs.

| Key | Value | Suggested Type | Required | Default |
| ------------- | ------------- | ------------- | ------------- | ------------- |
| `AWS_ACCESS_KEY_ID` | Your AWS Access Key. [More info here.](https://docs.aws.amazon.com/general/latest/gr/managing-aws-access-keys.html) | `secret env` | **Yes** | N/A |
| `AWS_SECRET_ACCESS_KEY` | Your AWS Secret Access Key. [More info here.](https://docs.aws.amazon.com/general/latest/gr/managing-aws-access-keys.html) | `secret env` | **Yes** | N/A |
| `AWS_S3_BUCKET` | The name of the bucket you're removing something. For example, `jarv.is` or `my-app-releases`. | `secret env` | **Yes** | N/A |
| `AWS_REGION` | The region where you created your bucket. Set to `us-east-1` by default. [Full list of regions here.](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-regions-availability-zones.html#concepts-available-regions) | `env` | No | `us-east-1` |

## License

This project is distributed under the [MIT license](LICENSE.md).
