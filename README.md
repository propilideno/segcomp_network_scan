# Segcomp - Network Scan and Analysis

## Tools
- tcpdump
- zeek
- ~~- cicflowmeter~~ nettopng
- nmap

## VM - AWS EC2

Adding ntopng to the mix. (insteadof cicflowmeter)
```bash
docker run -d --net=host ntop/ntopng:stable --community -i any
```

```bash
git clone https://github.com/propilideno/dpki
cd dpki
docker-compose up -d
```

```bash
docker run -d --name=grafana -p 3000:3000 grafana/grafana
docker run -d --name=mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123456 mysql
```

## Usage
```bash
IP=$(107.21.72.125)
```

With netcat you can check if a port is open or not.
```bash
for i in "80" "3000" "3306" "8080" "443" "22" "9000" ""; do nc -zv $IP $i; done
```
You can also use nmap to scan the ports.
```bash
```

### nmap
With nmap you can scan the ports of a target. You also can use the `-A` flag to get more information about the target and the `-p` flag to specify the ports you want to scan.
```bash
nmap $IP
nmap -A $IP
```

```bash
nmap -p0- -v -A -T4 $IP
nmap -p80-85,443,8000-8005,3000-3306 $IP
```

### tcpdump
Use tcpdump to capture packets from a network interface.
```bash
INTERFACE="enp0s5"
sudo tcpdump -i any # All interfaces
sudo tcpdump -i $(INTERFACE) # Interface filter

sudo tcpdump -i $(INTERFACE) port 80 # Interface, port filter
sudo tcpdump -i $(INTERFACE) port 80 -w target.pcap # Write to a pcap file
```
Read from a pcap file
```bash
sudo tcpdump -r -w target.pcap
```

### zeek
```bash
zeek -r mycapture.pcap
```
Parse the logs to CSV format.
```bash
cat *.log | sed 's/\t/,/g'
```
