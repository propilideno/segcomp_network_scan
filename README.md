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
```bash
nmap $IP
nmap -A $IP
nmap -p0- -v -A -T4 $IP
nmap -p80-85,443,8000-8005,3000-3306 $IP
``` 

### tcpdump
```bash
INTERFACE="enp5"
```

```bash
tcpdump -i $(INTERFACE) -w mycapture.pcap
``` 

### zeek
```bash
zeek -r mycapture.pcap
```
Parse the logs to CSV format.
```bash
cat *.log | sed 's/\t/,/g'
```
