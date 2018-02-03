# Reverse Proxy with Certificate Management

Many complex applications that consist of many individual parts are available as Docker images.
In order to be able to use such services safely in production, proper certificates are important.
They can be fumbled into such applications manually or the task can be delegated to two other Docker images that have specialized in it.

### Reverse Proxy

The Docker image [`jwilder/nginx-proxy`](https://hub.docker.com/r/jwilder/nginx-proxy/)
intercepts requests to web applications on the relevant ports 80 and 443 as reverse proxy and
forwards them to the appropriate container, determined by the container's name.

### Certificate Manager

The Docker image [`jrcs/docker-letsencrypt-nginx-proxy-companion`](https://hub.docker.com/r/jrcs/letsencrypt-nginx-proxy-companion/)
obtains a certificate for each name and provides it to the reverse proxy.
No extra steps are required for this on containers with the web application.

## Usage

## Configuration

### Environment Variables

All environment variables have sensible default values,
so specifying them should only be needed in some very special cases.

#### Reverse Proxy

##### `PROXY_IP`

The IP address of the reverse proxy. Defaults to `0.0.0.0`.

##### `PROXY_SERVER_NAME`

The container name for the reverse proxy. Defaults to `proxy_server`.

##### `PROXY_SERVER_LOG_MAX_SIZE`

The maximum log file size for the reverse proxy server. Defaults to 4MB.

##### `PROXY_SERVER_LOG_MAX_FILE`

The maximum number of log files for the reverse proxy server. Defaults to 10.

#### Certification Manager

##### `PROXY_CERT_MGR_NAME`

The container name for the reverse proxy. Defaults to `proxy_cert_mgr`.

##### `PROXY_CERT_MGR_LOG_MAX_SIZE`

The maximum log file size for the certificate manager. Defaults to 2MB.

##### `PROXY_CERT_MGR_LOG_MAX_FILE`

The maximum number of log files for the certificate manager. Defaults to 10.

##### `ACME_CA_URI`

To obtain unlimited (non-expiring) certificates for experiments, set `ACME_CA_URI` to  
`https://acme-staging.api.letsencrypt.org/directory`, for example by using the `-e` command line switch or adding it to the `proxy.yml` file in the `proxy_cert_mgr` section:

```yml
    environment:
      - ACME_CA_URI=https://acme-staging.api.letsencrypt.org/directory
```

#### Volumes

All changing data is kept in separate volumes to allow easy update of the images.

##### `PROXY_CERTS_PATH`

The path for the certificates. Defaults to `./certs`.

##### `PROXY_CONFIG_PATH`

The path for the reverse proxy configuration files. Defaults to `./conf.d`.

##### `PROXY_VHOSTS_PATH`

The path for the virtual hosts configurations. Defaults to `./vhosts.d`.

##### `PROXY_HTML_PATH`

The path for the html files. Defaults to `./html`.

#### Network

##### `PROXY_NETWORK`

The name of the proxy network. Defaults to `proxy_network`.
