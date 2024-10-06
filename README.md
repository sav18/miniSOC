# miniSOC
It is a Security Operation Center for incident response and risk identification.

## Test Environment Setup

### Installation and Configuration
#### Docker installation
Update the package list:
sudo apt-get update

Install Docker using the official Docker repository:
sudo apt-get install docker-ce docker-ce-cli containerd.io

Verify the Docker installation:
docker --version

#### Wazuh installation
Clone the Wazuh Docker repository:
git clone https://github.com/wazuh/wazuh-docker.git

Navigate to the repository directory and set up Wazuh using Docker Compose:
cd wazuh-docker
docker compose up 

Accesss Wazuh using the docker ip address
https://172.17.0.1/


Enter valid credentials to access the Wazuh Dashboard:
Create a Wazuh agent and activate the agent

#### IRIS Installation
This is not what I did
I just cloned the t-guard and installed all tools from there
THis is just for my report to show I did all the work hehe
Clone the iris-web repository
git clone https://github.com/dfir-iris/iris-web.git
cd iris-web

Checkout to the last tagged version
git checkout v2.4.12

Copy the environment file
cp .env.model .env

Build the dockers
docker compose build

Run IRIS
docker compose up -d

Accesss IRIS portal using the docker ip address
https://172.17.0.1:8443/

Enter valid credentials to access the IRIS Dashboard
Change the password for the admin user

#### MISP Installation
Clone the misp-docker GitHub repository:
git clone https://github.com/MISP/misp-docker.git

Change the MISP_BASEURL variable to reflect the IP address of the machine you are running MISP on

Build the Docker containers with the command:
docker-compose build

Accesss MISP using the docker ip address
https://172.17.0.1:1443/

Login with valid credentials

#### Shuffle Installation
Clone the shuffle Github repository:
git clone https://github.com/frikky/Shuffle

cd Shuffle

Build the Docker container:
docker-compose up -d

Accesss Shuffle using the docker ip address
https://172.17.0.1:3001/

Enter the default credentials for the admin user and access the Shuffle Dashboard:

Change the password for the administrator user

#### Verification
Verify all the components are running:
docker ps


## Integration
## Integration between Wazuh and IRIS
Obtain the IRIS API key from the IRIS settings. In the Wazuh manager configuration, enable the integration with IRIS by adding the IRIS API key and URL to the ossec.conf file/wazuh-manager.conf.

## Integration between Wazuh, IRIS and MISP
Set up the MISP API key in Wazuh's integration settings.
Modify the Wazuh manager configuration to include MISP's API key and URL.
Use IRIS as the central integration point, fetching alerts from Wazuh and correlating them with MISP data.

## Creating Workflows on Shuffle
Create a new workflow:
Use the Wazuh Node to receive alerts. This is where I am stuck at. I followed the steps suggested in the following articles and videos. But I was not able to create the workflow to receive the alerts from Wazuh.
Add a Condition Node to evaluate the alert severity. I have understood about how to use a condition but I am trying to figure out what parameters to put for checking the conditions: Here the condition is to check if there is any vulnerability. Eg. If there is a brute force attack then wazuh gets an alert saying ‘Multiple authentication failures’. In this condition send the alert to IRIS where it does a incident response and we can create a report for the incident. 
Integrate with IRIS and MISP using their respective nodes. MISP is a bit complicated so I chose not to implement it now, will focus on Shuffle.
Use nodes to send notifications to collaboration tools (e.g., Slack, email).
Save the workflow.


## Testing and Attack Simulation
### Testing the Integrations with Brute Force Attack
Use tools like Hydra or Metasploit to simulate a brute-force attack against an SSH service.
Trigger an alert in Wazuh (e.g., simulate a failed SSH login).
Verify the alert is correctly forwarded to IRIS.
Ensure that MISP pulls the relevant threat intelligence based on the Wazuh alert.


### Generating IRIS Report
I am also trying to understand how to do this as well
https://github.com/cudeso/dfir-iris-misp-timesketch

### Testing the Workflows
Run the workflow created in Shuffle to handle Wazuh alerts.
Monitor the workflow steps in Shuffle for correct processing and routing of alerts to IRIS and MISP.
Confirm that notifications are sent to the configured channels (e.g., Slack, email).
Once we figure out above steps then only we can progress to this (Sending the notification to email)

### Simulating Webshell Attack 
Deploy a test environment and introduce a malware sample.
Allow Wazuh to detect the malware infection.
Confirm that the detection is processed through IRIS and correlated with MISP's threat intelligence using the Shuffle workflow.
Check for automatic analysis using VirusTotal integration in Shuffle. 

### Simulating Sysjoker Malware Attack

### Analyzing Logs from Web Portal into Wazuh
Add this to the ossec.conf file to receive logs for apache, nginx
<localfile>
  <log_format>apache</log_format>
  <location>/var/log/apache2/access.log</location>
</localfile>

<localfile>
  <log_format>apache</log_format>
  <location>/var/log/apache2/error.log</location>
</localfile>

<!-- For Nginx Logs -->
<localfile>
  <log_format>nginx</log_format>
  <location>/var/log/nginx/access.log</location>
</localfile>

<localfile>
  <log_format>nginx</log_format>
  <location>/var/log/nginx/error.log</location>
</localfile>

<!-- For custom application logs -->
<localfile>
  <log_format>syslog</log_format>
  <location>/path/to/custom/app.log</location>
</localfile>



