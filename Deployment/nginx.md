# Nginx

## Support 

[*Open Source NGINX 1.9.5 Released with HTTP/2 Support*](https://www.nginx.com/blog/nginx-1-9-5/)

## Installtion(Ubuntu)

* Remove Old

  ```shell
  sudo apt-get remove '^nginx.*$'
  ```


* Source Code
  * Wget

    ```shell
    wget http://nginx.org/download/nginx-1.10.2.tar.gz
    ```

  * Tar

    ```shell
    tar -zxvf nginx-1.10.2.tar.gz
    ```

  * Install

    ```shell
    cd nginx-1.10.2
    ./configure --with-http_v2_module --with-http_ssl_module
    make && make install
    ```

* Pre-Built Packages

  * [nginx: Linux packages](http://nginx.org/en/linux_packages.html)

  * Add key

    ```shell
    wget http://nginx.org/keys/nginx_signing.key
    sudo apt-key add nginx_signing.key
    ```

  * Sources.list

    * append the following to the end of the `/etc/apt/sources.list`

      ```shell
      deb http://nginx.org/packages/ubuntu/ codename nginx
      deb-src http://nginx.org/packages/ubuntu/ codename nginx
      ```

    * Codename

      | Version | Codename | Supported Platforms         |
      | ------- | -------- | --------------------------- |
      | 12.04   | precise  | x86_64, i386                |
      | 14.04   | trusty   | x86_64, i386, aarch64/arm64 |
      | 16.04   | xenial   | x86_64, i386, ppc64el       |

    * Version

      ```shell
      cat /etc/issue
      ```

  * Update sources

    ```shell
    sudo apt-get update
    sudo apt-get install nginx
    ```

## Configuration

* Redirecting

  ```nginx
  server {
      listen 80;
      location / {
          return 301 https://$host$request_uri;
      }
  }
  ```

* Enabling

  ```nginx
  server {
      listen 443 ssl http2 default_server;

      ssl_certificate    server.crt;
      ssl_certificate_key server.key;
      ...
  }
  ```