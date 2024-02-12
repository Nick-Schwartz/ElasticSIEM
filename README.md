# ElasticSIEM
In this guide, I set up a home lab using Elastic SIEM and a Parrot Security OS Laptop. I forwarded data from the Parrot OS device to SIEM using Elastic Beats agent, generated security events on the Parrot OS laptop and also pulled log data from the router, and analyzed the logs in the SIEM using the Elastic web interface.

Elastic SIEM Home Lab

I came a across a video about setting up a home SIEM Lab which is based on the idea of setting up a Kali Linux VirtualBox and pushing telemetry to an elastic stack SIEM.
https://www.youtube.com/watch?v=2XLzMb9oZBI | Build a Powerful Home SIEM Lab Without Hassle! (Step by Step Guide) | 

I wanted to take it a step further though and give the SIEM a bit more information than what was provided in the tutorial. I thought it would be a great idea to set up a real
machine on my home network (running Parrot Sec OS) which is capable of running syslogs on my home network (router running pfSense software), and then pushing that telemetry into the stack. 

I have been wanting to find a way to monitor my home network after setting up my home router a while back. This way I can wake up in the morning and take a peek at any events that occured overnight and analyze the data received plus set up alerts accordingly. 

I hope you enjoy my version of this home lab, and would love any feedback you may have that would would help make this better!

Documentation:

 - Step 1: Get Elastic account.

 - Step 2: Setup a new integration >> Elastic Defend.

 - Step 3: Install Elastic Agent on your host
	curl -L -O https://artifacts.elastic.co/downloads/beats/elastic-agent/elastic-agent-8.12.1-linux-x86_64.tar.gz
	tar xzvf elastic-agent-8.12.1-linux-x86_64.tar.gz
	cd elastic-agent-8.12.1-linux-x86_64
	sudo ./elastic-agent install --url=https://6b791436ef6443ff8af350706ce83bc7.fleet.us-central1.gcp.cloud.es.io:443 --enrollment-token=M1lkX21JMEJUcmhxc3FFaTQ3RmQ6ZXpkSHc3cC1TUGFPakRQOGs2b0hlUQ==

 - Step 4: Tested the agent telemetry push to elastic by running nmap scans on localhost, two commands:
	nmap -p- localhost
	sudo nmap -sS localhost
    23:06:46.907
		endpoint.events.process
		Endpoint process event
		23:06:46.913
		endpoint.events.process
		Endpoint process event
		23:06:46.913
		endpoint.events.process
		Endpoint process event
		23:06:46.913
		endpoint.events.process
		Endpoint process event
		23:06:46.913
		endpoint.events.process
		Endpoint process event
		23:06:46.913
		endpoint.events.process
		Endpoint process event
		23:06:46.913
		endpoint.events.process
		Endpoint process event
		23:06:46.913
		endpoint.events.process
		Endpoint process event
		23:06:46.913
		endpoint.events.process
		Endpoint process event
		23:06:46.913
		endpoint.events.process
		Endpoint process event
		23:06:46.913
		endpoint.events.process
		Endpoint process event
		23:06:46.914
		endpoint.events.process
		Endpoint process event
		23:06:46.914
		endpoint.events.process
		Endpoint process event
		23:06:46.914
		endpoint.events.process
		Endpoint process event
		23:06:46.915
		endpoint.events.process
		Endpoint process event
		23:06:46.915
		endpoint.events.process
		Endpoint process event
		23:06:46.915
		endpoint.events.process
		Endpoint process event
		23:06:46.920
		endpoint.events.process
		Endpoint process event
		23:06:46.923
		endpoint.events.process
		Endpoint process event
		23:06:46.924
		endpoint.events.process
		Endpoint process event
		23:06:46.925
		endpoint.events.process
		Endpoint process event
		23:06:47.078
		endpoint.events.process
		Endpoint process event
		23:06:47.082
		endpoint.events.process
		Endpoint process event
	As we can see, the events populated in elastic, and we have the ability to open each one up and view further details. We can see the actual command ran in the evens.

 - Step 5: Set up syslogs on agent device:
	Installed rsyslog on system through CLI
	Edited /etc/rsyslog.conf to start listening on TCP/UDP port 514
	Restarted services using: sudo service rsyslog restart & sudo systemctl restart rsyslog
	Ran a scan to ensure the system is indeed listening on port 514 (confirmed screenshot)

 - Step 6: On pfSense router web interface I changed the system log settings to enable remote logging, selected LAN for source address, IPv4 protocol, input the IP of my device 		running rsyslog and specified port 514, then saved changes. Ran command: sudo cat /var/log/syslog | grep {10.27.27.1}. (confirmed screenshot)

 - Step 7: Setup Dashboard on Elastic. (screenshot)

 - Step 8: Created a rule to detect NMAP scans on the network (screenshot)

Conclusion: In this guide, I set up a home lab using Elastic SIEM and a Parrot Security OS Laptop. I forwarded data from the Parrot OS device to SIEM using Elastic Beats agent, generated security events on the Parrot OS laptop and also pulled log data from the router, and analyzed the logs in the SIEM using the Elastic web interface. I also created a dashboard to vizualize security events and then created an alert to detect security events, which will be sent to my web doman email address. 

This home lab provides a valuable environment for learning and practicing the skills necessary for effective security monitoring and incident response using Elastic SIEM. Through these steps, I gained hands-on experience with using a SIEM and improved my security monitoring skills.
