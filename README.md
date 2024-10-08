# Segcomp - Network Scan and Analysis

## Tools
- tcpdump
- zeek
- ~~- cicflowmeter~~ ntopng
- nmap

## VM - AWS EC2

We're using terraform to create a VM on AWS EC2. The VM is a t3.large instance with Ubuntu 20.04 LTS.
Terraform is a tool for building, changing, and versioning infrastructure safely and efficiently.

```bash
# this is a common terraform lifecycle
terraform init
terraform plan
terraform apply
terraform destroy
```

## Services
We're running these services on the VM, to analyze the target.

```bash
git clone https://github.com/propilideno/dpki
cd dpki
docker-compose up -d
```

This services are common used services in enterprise environments. We're using them because it's widely used and run on common ports.
```bash
docker run -d --name=grafana -p 8080:3000 grafana/grafana
docker run -d --name=mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123456 mysql
```

### Flow analysis

To analyze the flow of the network we're using ntopng. The command below will start the ntopng service on the VM.
ntopng is a web-based traffic analysis tool for monitoring networks based on flow data while providing useful insights into the network traffic.
```bash
docker run -d --net=host ntop/ntopng:stable --community -i any
```

## Usage
```bash
IP="54.162.185.143"
```
With netcat you can check if a port is open or not.
```bash
for i in "80" "3000" "3306" "8080" "443" "22" "9000"; do nc -zv $IP $i; done
```
You can also use nmap to scan the ports.

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
ip l # Show current interfaces
```
Generate a pcap file
```bash
INTERFACE="enp0s5"
sudo tcpdump -i any # All interfaces
sudo tcpdump -i $INTERFACE # Interface filter

sudo tcpdump -i $INTERFACE port 80 # Interface, port filter
sudo tcpdump -i $INTERFACE port 80 -w target.pcap # Write to a pcap file
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
