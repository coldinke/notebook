# 常用 Linux 网络相关工具

!!! abstract
    It should  work in Unix-like system.

## ping

Used to test network connectivity between hosts by sending ICMP echo request packets and measuring round-trip time (RTT).

syntax: `ping [options] host`

Common options:
- `-c`: Specify the count
- `-i`:
- `-s`: Specify the packet size.

Example:
```sh
ping -c 4 google.com
```

## traceroute

Displays the route the packets take from the source host to a destination host.

syntax: `traceroute [option] host`

Example:
```sh
traceroute google.com
```

## netstat

Show network connections, routing talbes, interface statistics, and more.

syntax: `netstat [options]`

Example:
```sh
netstat -tuln
```

## ifconfig/ip

Used to configure and display network interface information.

syntax: `ifconfg` or `ip addr show`

## curl

A tool for transferring data using various protocols, commonly used for making HTTP/HTTPS requests.

syntax: `curl [options] URL`

Example:
```sh
curl https://api.example.com/data
```

## wget

A utility for non-interactive download of files from the web.

syntax: `wget [options] URL`

Example:
```sh
wget https://example.com/file.zip
```

## ss

A utility to investigate sockets, similar to netstat but with more features.

syntax: `ss [options]`

Example:
```sh
ss -tuln
```

## tcpdump

A powerful command-line packate analyzer.

syntax: `tcpdump [options] [expression]`

Example:
```sh
tcpdump -i eth0 port 80
```

## nmap

## dig

## nslookup

## iptables

## ssh

## scp/rsync

## telnet