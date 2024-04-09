[![GitHub Workflow][1]](https://github.com/missuo/discord-image/actions)
[![Go Version][2]](https://github.com/missuo/discord-image/blob/main/go.mod)

[1]: https://img.shields.io/github/actions/workflow/status/missuo/discord-image/release.yaml?logo=github
[2]: https://img.shields.io/github/go-mod/go-version/missuo/discord-image?logo=go

Powerful image hosting / file sharing implemented using Discord Bot.

## Features
- Maximum supported single file size: 25MB.
- Files never expire.
- Support viewing upload history, support deleting file.
- Support for uploading images, videos, and other files.
- Support custom proxy url.
- Support automatic deletion of files after uploading to the server, will not occupy your server's hard disk.
- Support private deployment, secure and reliable.

## Config files and environment variables

You can leave `proxy_url` unset, but the domain of Discord cannot be accessed in mainland China. If you want to access Discord in mainland China, you must configure this option. How to deploy the proxy url, please continue to read below.

```yaml
bot:
  token: "" # Discord bot token
  channel_id: "" # Channel ID

upload:
  temp_dir: "uploads" # Temporary directory for storing files

proxy_url: example.com # Custom proxy url for cdn.discordapp.com
auto_delete: true # Automatically delete files after uploading to the server
```

```yaml
services:
  discord-image:
    images: ghcr.io/missuo/discord-image
    ports:
      - "8080:8080"
    environment:
      - BOT_TOKEN=your_bot_token
      - CHANNEL_ID=your_channel_id
      - UPLOAD_DIR=/app/uploads
      - PROXY_URL=your_proxy_url
      - AUTO_DELETE=true
    volumes:
      - ./uploads:/app/uploads
```

## Deploy proxy url

If you are using Nginx, you can use the following configuration:

```nginx
location /
{
    proxy_pass https://cdn.discordapp.com;
    proxy_set_header Host cdn.discordapp.com;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header REMOTE-HOST $remote_addr;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection $connection_upgrade;
    proxy_http_version 1.1;
}
```

Of course, you can use serverless tools like `Cloudflare Workers` to deploy Proxy URL.

## License
AGPL-3.0