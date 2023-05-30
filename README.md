# fileshare 

Easy peer-to-peer file transfer.

<p align="center">
        <img src="https://raw.githubusercontent.com/raghhavdturki/fileshare/main/client/fileshare.gif" alt="Screenshot">
    </a>
</p>

## Client

### Installation

Run `yarn install`, `yarn build` and then simply run `yarn start` to start the server.

#### Environment variables

The following variables are used in the build process:

| Variable                       | Default value             | Description                                                                 |
| ------------------------------ | ------------------------- | --------------------------------------------------------------------------- |
| `REACT_APP_TITLE`              | `filedrop`                | Application title.                                                          |
| `REACT_APP_SERVER`             | `ws://[hostname]:5000/ws` | WebSockets server location.                                                 |
| `REACT_APP_USE_BROWSER_ROUTER` | `0`                       | `1` if you want the application to use BrowserRouter instead of HashRouter. |
| `REACT_APP_ABUSE_EMAIL`        | null                      | E-mail to show in the Abuse section.                                        |

## Server

### Installation

Run `yarn install`, `yarn build` and then simply run `yarn start`. For development you can also run filedrop-ws with live reload, `yarn dev`. You can also test the server with `yarn test`.

### Configuration

`dotenv-flow` is used to manage the configuration.

The following variables are used for the configuration:

| Variable          | Default value                   | Description                                                                       |
| ----------------- | ------------------------------- | --------------------------------------------------------------------------------- |
| `WS_HOST`         | `127.0.0.1`                     | IP address to bind to.                                                            |
| `WS_PORT`         | `5000`                          | Port to bind to.                                                                  |
| `WS_BEHIND_PROXY` | `no`                            | Set to `yes` if you want the application to respect the `X-Forwarded-For` header. |
| `WS_MAX_SIZE`     | `65536`                         | The limit should accommodate preview images (100x100 thumbnails).                 |
| `STUN_SERVER`     | `stun:stun1.l.google.com:19302` | STUN server address.                                                              |
| `TURN_MODE`       | `default`                       | `default` for static credentials, `hmac` for time-limited credentials.            |
| `TURN_SERVER`     | null                            | TURN server address.                                                              |
| `TURN_USERNAME`   | null                            | TURN username.                                                                    |
| `TURN_CREDENTIAL` | null                            | TURN credential (password).                                                       |
| `TURN_SECRET`     | null                            | TURN secret (required for `hmac`).                                                |
| `TURN_EXPIRY`     | `3600`                          | TURN token expiration time (when in `hmac` mode), in seconds.                     |
| `NOTICE_TEXT`     | null                            | Text of the notice to be displayed for all clients.                               |
| `NOTICE_URL`      | null                            | URL the notice should link to.                                                    |


## Self-hosting

A docker-compose configuration is available in the [docker-compose.yml](https://github.com/RaghhavDTurki/fileshare/blob/main/docker-compose.yml) file.

Installation can be achieved without Docker as well:

> First you need to clone this project, build and run the server [server](https://github.com/RaghhavDTurki/fileshare/tree/main/server/) and a TURN server (like [coturn](https://github.com/coturn/coturn)), read the README in filedrop-ws for more information on configuration.
>
> Then you need to point the client to the WebSockets backend (fileshare-server) (in .env.local), build it and place it on some static file server (I use nginx for that). I also use nginx to proxy the back end through it. [Here's a guide on how to achieve that.](https://www.nginx.com/blog/websocket-nginx/)

## FAQ

### What is the motivation behind the project?

I didn't feel comfortable logging into my e-mail account on devices I don't own just to download an attachment and cloud services have extremely long URLs that aren't really easy to type.

### Where do my files go after I send them through the service?

To the other device. Sometimes the (encrypted, since WebRTC uses encryption by default) data goes through the TURN server I run. It's immediately discarded after being relayed. File metadata also is not saved.

### Doesn't this exist already?

While [ShareDrop](https://github.com/cowbell/sharedrop) and [SnapDrop](https://github.com/RobinLinus/snapdrop) are both excellent projects and most definitely exist, I felt the need to create my own version for a several reasons:

- I wanted to build something using TypeScript.
- ShareDrop doesn't work when the devices are on different networks but still behind NAT.
- I didn't like the layout and design of both, I feel like the abstract design of FileDrop makes it easier to use.
- I was not aware of these projects while I started working on this project.
- ShareDrop's URLs are extremely long.