# 🔒 Vulcan

**Vulcan** is a lightweight Python daemon designed to synchronize SSH public keys from an Amazon S3 bucket to your servers. By simply adding or removing SSH keys in the S3 bucket, you can grant or revoke access to multiple servers at once, making access management seamless and efficient.


## Installation

Get the latest version of **Vulcan** directly from Github, and ensure it is executable:
```bash
wget https://github.com/davidecaroselli/vulcan-ssh/releases/download/v1.0.0/vulcan && chmod +x vulcan
```

You can place the binary in a directory of your choice, in this example we will move it to `/usr/local/bin`:
```bash
mv vulcan /usr/local/bin
```

### Verify Installation

You can verify the installation by running the following command:
```bash
vulcan --version
```

## Cron Job Setup

To keep the SSH keys in sync, you can set up a cron job to run the `vulcan` command at regular intervals.
In order to let the daemon access the S3 bucket, you need to provide the necessary AWS credentials.
You can do this by setting the `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY` environment variables in the cron job.

This is an example of a cron job that runs the `vulcan` command every 30 minutes:
```bash
sudo tee /etc/cron.d/vulcan-ssh > /dev/null <<EOF
AWS_ACCESS_KEY_ID=<your_access_key>
AWS_SECRET_ACCESS_KEY=<your_secret_key>

*/30 * * * * ubuntu /usr/local/bin/vulcan <your_bucket_name> 2>&1 | /usr/bin/logger -t vulcan-ssh
EOF
```