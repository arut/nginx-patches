# Nginx patches

## dav-copy-hardlink

Implements hardlinking in DAV copy instead of real copy.

    dav_copy_hardlink on;

## per-worker-listener

Implements per worker listeners to make requests to
certain nginx worker. This requires `accept_mutex off`;
This patch only works on POSIX systems (Linux, FreeBSD etc).

    worker_processes 5;

    events {
        # required by per_worker
        accept_mutex off;
    }
    ...
    http {
        ...
        server {
            # usual listener
            listen 80;

            # per-worker listener
            # 1st worker will listen 8000
            # 2nd worker will listen 8001
            # 3rd worker will listen 8002
            # 4th worker will listen 8003
            # 5th worker will listen 8004
            listen 8000 per_worker;
            ...
        }
    }
