worker_processes 1;
include /etc/nginx/modules-enabled/*.conf;



events {
	worker_connections 1024;
}

http {
	include mime.types;
	default_type application/octet-stream;
	sendfile on;
	keepalive_timeout 65;
	server {
		listen 80;
		server_name localhost;
		location / {
			root html;
			index index.php index.html index.htm;
		}
		location /live {
			types {
				application/vnd.apple.mpegurl m3u8;
			}
			alias /home/hls/live;
			add_header Cache-Control no-cache;
		}
		location /dash {
			alias /home/dash/live;
			add_header Cache-Control no-cache;
		}
	}
}

rtmp {

	server {
		listen 1935;
		chunk_size 8192;
		
		application live {
			live on;
			allow publish 127.0.0.1;
			allow publish all;
			allow play all;
			record all;
			record_path /home/video_recordings;
			record_unique on;
			hls on;
			hls_nested on;
			hls_path /home/hls;
			hls_fragment 10s;
			dash on;
			dash_path /home/dash;
			dash_nested on;
		}

		# Video on Demand
		application vod {
			play /home/vod;
		}

		# Restream
		application restream {
			live on;
			# push server1:1935
			# push server2:1935
		}

	}

}