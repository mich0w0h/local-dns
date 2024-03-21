FROM ubuntu:22.04

RUN apt-get update && apt-get install -y unbound && rm -rf /var/lib/apt/lists/*

COPY unbound.conf /etc/unbound/unbound.conf

CMD ["/usr/sbin/unbound", "-d", "-c", "/etc/unbound/unbound.conf"]
