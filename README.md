What is Metabigor?
Metabigor is Intelligence tool, its goal is to do OSINT tasks and more but without any API key.

Installation
Pre-built Binaries
You can download pre-built binaries for your platform from the releases page. Choose the appropriate binary for your operating system and architecture, download it, and place it in your PATH.

Building from Source
go install github.com/j3ssie/metabigor@2.0.0
Main features
Searching information about IP Address, ASN and Organization.
Wrapper for running rustscan, masscan and nmap more efficient on IP/CIDR.
Finding more related domains of the target by applying various techniques (certificate, whois, Google Analytics, etc).
Get Summary about IP address (powered by @thebl4ckturtle)
Usage
Discovery IP of a company/organization - metabigor net
The difference between net and netd command is that netd will get the dynamic result from the third-party source while net command will get the static result from the database.

# discovery IP of a company/organization
echo "company" | metabigor net --org -o /tmp/result.txt

# discovery IP of an ASN
echo "ASN1111" | metabigor net --asn -o /tmp/result.txt
cat list_of_ASNs | metabigor net --asn -o /tmp/result.txt

echo "ASN1111" | metabigor netd --asn -o /tmp/result.txt
Finding more related domains of the target by applying various techniques (certificate, whois, Google Analytics, etc) - metabigor related
Note some of the results are not 100% accurate. Please do a manual check first before put it directly to other tools to scan.

Some specific technique require different input so please see the usage of each technique.

Using certificate to find related domains on crt.sh
# Getting more related domains by searching for certificate info
echo 'Target Inc' | metabigor cert --json | jq -r '.Domain' | unfurl format %r.%t | sort -u # this is old command

# Getting more related domains by searching for certificate info
echo 'example Inc' | metabigor related -s 'cert'
Wrapper for running rustscan, masscan and nmap more efficient on IP/CIDR - metabigor scan
This command will require you to install masscan, rustscan and nmap first or at least the pre-scan result of them.

# Only run rustscan with full ports
echo '1.2.3.4/24' | metabigor scan -o result.txt

# Only run nmap detail scan based on pre-scan data
echo '1.2.3.4:21' | metabigor scan -s
cat list_of_ip_with_port.txt | metabigor scan -c 10 --8 -s -o result.txt
cat list_of_ip_with_port.txt | metabigor scan -c 10 --tmp /tmp/raw-result/ -s -o result.txt
echo '1.2.3.4 -> [80,443,2222]' | metabigor scan -R

# Run rustscan with full ports and nmap detail scan based on pre-scan data
echo '1.2.3.4/24' | metabigor scan --pipe | metabigor scan -R 
Using Reverse Whois to find related domains
echo 'example.com' | metabigor related -s 'whois'
Getting more related by searching for Google Analytics ID
# Get it directly from the URL
echo 'https://example.com' | metabigor related -s 'google-analytic'

# You can also search it directly from the UA ID too
metabigor related -s 'google-analytic' -i 'UA-9152XXX' --debug
Get Summary about IP address (powered by @thebl4ckturtle) - metabigor ipc
This will show you the summary of the IP address provided like ASN, Organization, Country, etc.

cat list_of_ips.txt | metabigor ipc --json
Extract Shodan IPInfo from internetdb.shodan.io
echo '1.2.3.4' | metabigor ip -open
1.2.3.4:80
1.2.3.4:443

# lookup CIDR range
echo '1.2.3.4/24' | metabigor ip -open -c 20
1.2.3.4:80
1.2.3.5:80

# get raw JSON response
echo '1.2.3.4' | metabigor ip -json
Demo
asciicast

Painless integrate Metabigor into your recon workflow?
OsmedeusEngine

This project was part of Osmedeus Engine. Check out how it was integrated at @OsmedeusEngine

Credits
Logo from flaticon by freepik

Disclaimer
This tool is for educational purposes only. You are responsible for your own actions. If you mess something up or break any laws while using this software, it's your fault, and your fault only.

License
Metabigor is made with ♥ by @connectv01d and it is released under the MIT license.
