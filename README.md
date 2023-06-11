# Cloud-Based Automation System
# SCOPE
The proposed cloud-based base-station system will support 400 satellites per scaled cluster.  Each cluster will consist of one satellite terminal, one web server, one database server, and one time server.  To provide a smooth and lag-free experience, once the active cluster has reached 300 satellites, a scaled cluster will be created to support the growing demand.
To reduce costs and provide more flexibility, the scaled clusters are configured with single-core VMs, supporting up to 300 satellites.  If the scaled cluster has reached 225 satellites, an additional scaled cluster will be created.  With the scaled clusters configured with a single-core, cost savings will be gained from the reduced infrastructural overhead.  In addition, since the scaled clusters support a smaller group of satellites, SAd INC will be able to scale more incrementally rather than regenerating clusters that are duplicates of the primary cluster.
In addition to scaling based on satellite usage, the proposed solution has the capability to scale based on time of day and geographical location that is either predetermined based on prior campaign history or based on a new and upcoming advertisement campaign. 
	
# FUNCTIONALITY
	The tools used to deploy and operate this proposed solution are as follows…
1.	Chef – Automation tool
a.	Kitchen.yml – Preconfigured automation script
2.	Vagrant – Orchestration tool
3.	Virtualbox – Virtual machine creation tool 
	Using Chef as the automation tool, policies and configurations are established to provide guidelines and structure for how virtual machines are created.  Chef will call on a kitchen.yml script, which stores the deployment configuration in a scripted language, and deploys it automatically when required.  Chef ensures that standards and the appropriate state of the environment are constantly maintained.
	Vagrant is used to orchestrate and manage the provisioning of virtual machines to generate the environment in this solution.  Vagrant will then utilize Virtualbox as the software that performs the creation of virtual machines.
	With the combination of Chef, Vagrant, & Virtualbox tools, the proposed solution will ensure that advertisement campaigns utilizing the new GEN-2 satellites will be a smooth and seamless experience for the targeted audience and financially productive for SAd INC. leading to the potential for tremendous growth over the coming years.

# VISUAL REPRESENTATION
![image](https://github.com/CJeys/Automation-and-Scaling-Tools/assets/126273127/1dbc7f25-b747-46ef-a04c-68150e0ad173)
 
Automation VM running Vagrant with kitchen.yml script consists of the following VM devices:
•	Core VM Cluster – supports up to 400 satellites
  o	Satellite terminal server
  o	Web server
  o	Database server
  o	Time server
•	Autoscaling VM Cluster - supports up to 300 satellites
  o	Scaled satellite terminal server
  o	Scaled web server
  o	Scaled database server
  o	Scaled time server
Core VM Cluster will run 24x7 throughout the advertising campaign.
When the satellite usage reaches 300 satellites in the Core VM Cluster, the Autoscaling VM Cluster will be deployed automatically.
When overall satellite usage returns to below 300 satellites, Autoscaling VM Cluster will be deactivated.
Reporting – Activation and deactivation alerts of each new cluster and error conditions will send a message to the help desk ticketing system via helpdesk@satelliteads.com.

# AUTOMATION SCRIPT

#- Vagrant is the orchestration being used
driver:
  name: vagrant

#- Chef is the automation tool being used
provisioner:
  name: chef_zero
  product_name: chef
  product_version: 14.12.9

#- InSpec is being used for testing and auditing
verifier:
  name: inspec

#- CentOS 7 is the operating system utilized for the virtual machines
platforms:
  - name: centos-7
    customize:
      memory: 512

#- The following is a list of virtual machines to be created
#- Satellite Terminal Server (TERM)
#- Master Time Server (NTP)
#- Web Server (WEB)
#- Database Server (DBS)
#- Scaled Satellite Terminal Server (STERM)
#- Scaled Time Server (SNTP)
#- Scaled Web Server (SWEB)
#- Scaled Database Server (SDBS)

suites:
  - name: Satellite_Terminal_Server-TERM # Satellite Terminal Server (TERM)
    driver:
      vm_hostname: TERM.satelliteads.com
      network:
        - ["public_network"] # creates interface to allow VMs to reach each other
    run_list:
      - recipe[learn_chef_httpd::default]
    attributes:
  - name: Master_Time_Server-NTP # Master Time Server (NTP)
    driver:
      vm_hostname: NTP.satelliteads.com
      network:
        - ["public_network"] # creates interface to allow VMs to reach each other
    run_list:
      - recipe[learn_chef_httpd::default]
    attributes:
  - name: Web_Server-WEB # Web Server (WEB)
    driver:
      vm_hostname: WEB.satelliteads.com
      network:
        - ["public_network"] # creates interface to allow VMs to reach each other
    run_list:
      - recipe[learn_chef_httpd::default]
    attributes:
  - name: Database_Server-DBS # Database Server (DBS)
    driver:
      vm_hostname: DBS.satelliteads.com
      network:
        - ["public_network"] # creates interface to allow VMs to reach each other
    run_list:
      - recipe[learn_chef_httpd::default]
    attributes:
  - name: Scaled_Satellite_Terminal_Server-STERM # Scaled Satellite Terminal Server (STERM) 
    driver:
      vm_hostname: STERM.satelliteads.com
      network:
        - ["public_network"] # creates interface to allow VMs to reach each other
    run_list:
      - recipe[learn_chef_httpd::default]
    attributes:
  - name: Scaled_Master_Time_Server-SNTP # Scaled Time Server (SNTP)
    driver:
      vm_hostname: SNTP.satelliteads.com
      network:
        - ["public_network"] # creates interface to allow VMs to reach each other
    run_list:
      - recipe[learn_chef_httpd::default]
    attributes:
  - name: Scaled_Web_Server-SWEB # Scaled Web Server (SWEB)
    driver:
      vm_hostname: SWEB.satelliteads.com
      network:
        - ["public_network"] # creates interface to allow VMs to reach each other
    run_list:
      - recipe[learn_chef_httpd::default]
    attributes:
  - name: Scaled_Database_Server-SDBS # Scaled Database Server (SDBS)
    driver:
      vm_hostname: SDBS.satelliteads.com
      network:
        - ["public_network"] # creates interface to allow VMs to reach each other

  ![image](https://github.com/CJeys/Automation-and-Scaling-Tools/assets/126273127/6850d57c-3b45-4d07-9206-4d79f4b2c49d)
  
