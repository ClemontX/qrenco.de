## Usage

The service is used to generate QR-codes for strings in a UNIX/Linux console
using curl/httpie/wget or similar tools. 

```
	$ curl qrenco.de

	Append the string that you want to encode to qrenco.de/ in the URL
	or pipe the string into curl -F

										   $ printf "two\nlines" \
	$ curl qrenco.de/this                  | curl -F-=\<- qrenco.de

	█████████████████████████████          █████████████████████████████
	█████████████████████████████          █████████████████████████████
	████ ▄▄▄▄▄ █▀ █ ▄█ ▄▄▄▄▄ ████          ████ ▄▄▄▄▄ ███▄█ █ ▄▄▄▄▄ ████
	████ █   █ █▄ █▀▄█ █   █ ████          ████ █   █ █▄█▄█▀█ █   █ ████
	████ █▄▄▄█ █ ██▀ █ █▄▄▄█ ████          ████ █▄▄▄█ ██ ▀ ▄█ █▄▄▄█ ████
	████▄▄▄▄▄▄▄█ ▀ ▀ █▄▄▄▄▄▄▄████          ████▄▄▄▄▄▄▄█ █ █▄█▄▄▄▄▄▄▄████
	████ ▄█ ▀▀▄▄▀▄▄███ ▄▄▀  ▀████          ████ ▄ █▄ ▄  ▀▄▀▄▄▄▀▀█▄█▀████
	████ █▄ ▀█▄▄ █▄▀▄▄███▀▄▀ ████          ████▀ ██▀ ▄█ ▄█ ▀▄▄  ▄█ ▄████
	█████████▄▄█▀  ▀▄█▄▀ ▀▀  ████          ████▄█▄▄█▄▄█▀█▀▄█ █ ▀█▀▄ ████
	████ ▄▄▄▄▄ █▄  ▀▀ █ ▀█▄▄█████          ████ ▄▄▄▄▄ █ ▄▄█▄ █▄▄ █  ████
	████ █   █ █▀▄▀ ██▄ ▄▄█▀ ████          ████ █   █ █▄█ ▀▄▀ ▀▀▀ ▀█████
	████ █▄▄▄█ █▀█▄█▄▀▀▀▄ ██▄████          ████ █▄▄▄█ █ ▄▄ ▀▀█ ▄██▀▄████
	████▄▄▄▄▄▄▄█▄▄▄▄▄████▄▄▄█████          ████▄▄▄▄▄▄▄█▄█▄▄██▄▄███▄▄████
	█████████████████████████████          █████████████████████████████
	█████████████████████████████          █████████████████████████████

```

The service uses [libqrencode](https://github.com/chubin/qrenco.de) to generate QR-codes.

## Installation 

```
	$ git clone https://github.com/chubin/qrenco.de
    $ cd qrenco.de
	$ virtualenv ve
	$ ve/bin/pip install -r requirements.txt
	$ sudo apt-get install libqrenv
	$ ve/bin/python bin/srv.py
```

If you want to use a HTTP-frontend for the service,
configure it this way:

```
server {
    listen 80;
    listen [::]:80;
    server_name  qrenco.de *.qrenco.de;
    access_log  /var/log/nginx/qrenco.de-access.log;
    error_log  /var/log/nginx/qrenco.de-error.log;

    location / {
        proxy_pass         http://127.0.0.1:8003;

        proxy_set_header   Host             $host;
        proxy_set_header   X-Real-IP        $remote_addr;
        proxy_set_header   X-Forwarded-For  $remote_addr;

        client_max_body_size       10m;
        client_body_buffer_size    128k;

        proxy_connect_timeout      90;
        proxy_send_timeout         90;
        proxy_read_timeout         90;

        proxy_buffer_size          4k;
        proxy_buffers              4 32k;
        proxy_busy_buffers_size    64k;
        proxy_temp_file_write_size 64k;

        expires                    off;
    }

}

```
