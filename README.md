[![Build Status](https://travis-ci.org/evertrue/s3_dir.svg?branch=master)](https://travis-ci.org/evertrue/s3_dir)
[![Coverage Status](https://img.shields.io/coveralls/evertrue/s3_dir.svg)](https://coveralls.io/r/evertrue/s3_dir)

# s3_dir

This cookbook leverages the [`s3_file` cookbook](https://supermarket.getchef.com/cookbooks/s3_file) to recursively download all of the files in a specified S3 “directory.” It uses the [`et_fog` cookbook](https://supermarket.getchef.com/cookbooks/et_fog) cookbook to retrieve a listing of said “directories”.

## Usage

Firstly, you will need to include `et_fog` in your recipe in order to install the Fog library in Chef, e.g.:

```ruby
include_recipe 'et_fog'
```

### `s3_dir` Resource

```ruby
s3_dir '/local/path' do
  bucket            's3.bucket.name'
  dir               's3_directory'
  owner             'root'
  group             'root'
  mode              '0755'
  recursive         true
  access_key_id     'ACCESS_KEY'
  secret_access_key 'SECRET_ACCESS_KEY'
end
```

* `name` - The local path where the files should be downloaded to
* `bucket` - The S3 bucket name you want to download from
* `dir` - The directory within the bucket where the files will come from
* `owner` - What you want the local file/directory owner to be set to. Defaults to `root`.
* `group` - What you want the local file/directory group to be set to. Defaults to `root`.
* `mode` - File permissions.  Created directories will inherit these permissions plus executability. Defaults to '0755'
* `recursive` - Whether parent directories of `name` should be created recursively
* `access_key_id` - AWS Access Key ID
* `secret_access_key` - AWS Secret Key

## AWS Permissions

The following minimum AWS permissions are required for `s3_dir` to work:

```json
"Action": [
  "s3:GetObject",
  "s3:ListBucket"
],
"Resource": [
  "arn:aws:s3:::bucket-name",
  "arn:aws:s3:::bucket-name/*"
]
```

## Contributing

1. Fork the repository on Github
2. Create a named feature branch (i.e. `add-new-recipe`)
3. Write your change
4. Write tests for your change (if applicable)
5. Run the tests, ensuring they all pass
6. Submit a Pull Request

## License and Authors

Author:: EverTrue, Inc. (<devops@evertrue.com>)
