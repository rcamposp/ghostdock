# Ghostdock

## Development
To deploy your Ghost blog locally just run:

`docker-compose up`

This will use the default configuration:
* http://localhost as your blog host.
* Run Ghost in development mode.
* **HTTS Support is not allowed locally.**

## Production
For production we need additional setup:

### Prerequisites
* VPS with Docker Compose installed (Amazon's EC2 is a good option).
* Domain name (check http://freenom.com for free domains).


### Seting up production mode
Once you have your VPS up and runing and your domain name pointing to it, you are ready to deploy your blog using Ghostdock.

* In docker-compose.yml comment these lines:

```
NODE_ENV: development

- ./caddy/Caddyfile.local:/etc/Caddyfile
```

* In docker-compose.yml uncomment these lines:

```
NODE_ENV: production

- ./caddy/Caddyfile:/etc/Caddyfile
```

* In ./caddy/Caddyfile change **https://example.com** to your corresponding domain name (make sure to add the "s" at "https". And **bob@example.com** to your own email. This configuration is needed for caddy to automatacally setup your Let's Encrypt SSL Certificate.

* Once you run `docker-compose up` for the first time, Docker will download from the Ghost repository, all the files for your blog. The last thing we need to setup is the Ghost configuration file: **./ghost/config.js**. Inside the `production` block edit the `url` param to point to your own domain. Then add this block:

```
paths: {
    contentPath: path.join(process.env.GHOST_CONTENT, '/')
}
```

Now you are good to go. Run:
```
docker-compose up -d
```

Enjoy ^^