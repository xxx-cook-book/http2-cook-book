# Nginx

## Support 

[*Open Source NGINX 1.9.5 Released with HTTP/2 Support*](https://www.nginx.com/blog/nginx-1-9-5/)

## Installtion(Ubuntu)

* Remove Old Nginx

  ```shell
  sudo apt-get remove '^nginx.*$'
  ```

* Upgrade OpenSSL

  * See below

* Install Latest Nginx

  * Source Code

    * Wget

      ```shell
      wget http://nginx.org/download/nginx-1.10.2.tar.gz
      ```

    * Tar

      ```shell
      tar -zxvf nginx-1.10.2.tar.gz
      ```

    * PCRE Lib

      ```shell
      apt-get install libpcre3 libpcre3-dev
      ```

    * Install

      ```shell
      cd nginx-1.10.2
      ./configure --with-http_ssl_module --with-http_v2_module --with-http_realip_module --with-http_flv_module --with-http_mp4_module --with-ipv6 --with-openssl=/xxx/openssl-1.0.2j --with-http_stub_status_module
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

  * Ubuntu PPA

    * [3rd Party Repository: Nginx](http://www.ubuntuupdates.org/ppa/nginx)

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

## Why HTTP/2 May not Work in Chrome

* Reason
  * [Transitioning from SPDY to HTTP/2](https://blog.chromium.org/2016/02/transitioning-from-spdy-to-http2.html)

* Solution
  * Nginx with OpenSSL 1.0.2 (ALPN)

* Upgrade OpenSSL

  * [Upgrade OpenSSL on Ubuntu 12.04](http://askubuntu.com/questions/429385/upgrade-openssl-on-ubuntu-12-04)

  * [Updating To OpenSSL 1.0.2g On Ubuntu Server 12.04 & 14.04 LTS To Stop CVE-2016-0800 (DROWN attack)](http://www.miguelvallejo.com/updating-to-openssl-1-0-2g-on-ubuntu-server-12-04-14-04-lts-to-stop-cve-2016-0800-drown-attack/)

    ```shell
    $ curl https://www.openssl.org/source/openssl-1.0.2j.tar.gz | tar xz && cd openssl-1.0.2j && sudo ./config && sudo make && sudo make install
    $ which openssl
    /usr/bin/openssl
    $ sudo ln -sf /usr/local/ssl/bin/openssl /usr/bin/openssl
    $ openssl version
    OpenSSL 1.0.2j  26 Sep 2016
    ```

  * ALPN Support Or Not

    ```shell
    $ openssl s_client -alpn h2 -servername lzw.me -connect lzw.me:443 < /dev/null | grep 'ALPN'
    ALPN protocol: h2
    ```

## References

[1] victor@4devs, [Nginx's HTTP/2 Does Not Work](https://victor.4devs.io/en/architecture/nginx-http2-does-not-work.html)

[2] Sherin Abdulkhareem@syslint, [How to enable ALPN with http2 for nginx on Centos 7 cPnginx Server](https://syslint.com/blog/tutorial/how-to-enable-alpn-with-http2-for-nginx-on-centos-7-cpnginx-server/)