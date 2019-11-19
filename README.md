# Web application Security Documentation


I started my project by doing some penetration testing.
To do so i had to set up the right environment, i was recommened using a Linux/Unix machine when it comes to pentesting
purely for the terminal and sheer number of tools available. 

<h1>Setting up Kali Linux and Pentester VM</h1>

<h2>Step 1 - Install Kali Linux</h2>

 Visit the link below: 
 https://www.kali.org/downloads/
 Install the appropriate Image, I downloaded the Kali Linux 64-Bit Version 2019.3
 
 
 <img src="Images/KaliLinuxImage.PNG">
 

 
Step 2 - Open Oracle VM to deploy the Kali Linux Image

<h2>Step 2 - Open Oracle VM to deploy the Kali Linux Image</h2>


 - Click New
 - Enter a Name
 - Type "Linux"
 - Version: Pick your Kali linux image version
 - Click Next
 - Allocate Memory Size: I used 8GB
 - Select "Create a virtual hard disk now" and click "Create"
 - Select VDI and go "Next"
 - Select "Dynamically allocated"
 - Allocate hard disk size, I chose 20GB
 - Click "Create"
 - Right click the recently created VM and click "Settings"
 - Click storage and click the "Add optical drive icon"
 - Select the recently downloaded Kali Linux Image then click "OK"
 - Run the VM and navigate through the steps to personalize your new Kali Linux machine.

<h2>Step 3 - Get the pentester VM running to begin pentesting</h2>

 - Follow the link below:
 - https://www.pentesterlab.com/exercises/web_for_pentester/attachments
 - Download the ISO 
 - Open Oracle VM
 - Create a new VM using the steps used in "Step 2"
 - Right click the recently created VM and click "Settings"
 - Click storage and click the "Add optical drive icon"
 - Select the recently downloaded pentester ISO then click "OK"
 - Run through the setup process of the new VM
 - Once the terminal has loaded for the new VM use the "ifconfig" command
 - Take note of the inet addr listed under eth0
 
 
 <img src="Images/PentesterIP.PNG">
 
 
 - Enter this IP into your Kali Linux Browser to start using the Pen tester web applications.
 
 
 <img src="Images/PentesterPage.PNG">
 

 
Installing ZAP on Linux AND windows

<h1>Installing ZAP on Linux AND windows</h1>


<h2>Step 1 - Installing ZAP on Kali Linux machine</h2>

 - Open the Kali Linux Terminal and enter the following command:
 - wget https://github.com/zaproxy/zaproxy/wiki/Downloads
 - Add the correct permissions which allow us to execute the installer:
 - chmod u+x ZAP_2_8_0_unix.sh
 - Run the installer using sudo
 - sudo ./ZAP_2_8_0_unix.sh
 - Follow the instructions on the install wizard. When prompted, select "Standard Installation"
 - Use the ZAP tool by using the following command in the terminal
 - "zap.sh"
 - A dialogue box will appear as displayed below:
 
 
 <img src="Images/ZAPdialogue.PNG">
 
 
 - Select "Yes, I want to persist this session but I want to specify the name and location."
 
<h2>Step 2 - Installing ZAP on Windows</h2>

 - Follow the link below:
    https://github.com/zaproxy/zaproxy/wiki/Downloads
 - Find the appropriate link dependant on your Windows version

<h1>Using ZAP to run basic scans on web applications</h1>

 - To perform a quick scan using ZAP on any web URL
 - Click the big "Automated Scan" button
 
 
 <img src="Images/Zapquickscan.PNG">
 
 
  - Enter a URL in the "URL to attack column"
  - Click "Attack"
  - I recommend using one of the pentester URLS and working through these to test the different results from each
  - Once the scan has been completed click on the Alerts tab down the bottom of the screen shown below:
  
  
  <img src="Images/Alerts.PNG">
  
  
  - When looking through the Alerts folder keep in mind that Red is often a serious Vulnerability while they get less
  and less serious as the flags get lighter in colour.
  - Click the small arrow beside the red flagged address
  
  
  <img src="Images/RedFlag.PNG">
  
  
  - Look through the output and review through the information on the right hand pane.
  
  
  <img src="Images/RedFlagPane.PNG">
  
  
  - Important to note information is the "Attack:" info which shows a ready to use exploit on which you can use on a vulnerable parameter.
  - In the case shown above using the "</html><script>alert(1);</script><html>" script on the name parameter will expose a 
    exploit in the web application. 
  - Working through each of the Pentester web applications and testing the different results is a good learning convention.
  - Something to note: 
    - ZAP does not find 100% of vulnerabilities on a web application and there are often many vulnerabilities that can be missed.
	- This means having your own hard coding knowledge on vulnerabilities will come in handy
  - Useful link for more information on ZAP is linked below
  
  https://resources.infosecinstitute.com/introduction-owasp-zap-web-application-security-assessments/#gref
  
  <h1>Using ZAP baseline script with docker</h1>
  
  - On kali linux machine terminal install docker using the command "apt-get install docker-ce"
  - To ensure docker service has been enable run the command "systemctl start docker"
  - Use "docker pull owasp/zap2docker-stable" or "docker pull owasp/zap2docker-weekly" 
  To run ZAP in headless mode with the stable script previously pulled use:
  - docker run -u zap -p 8080:8080 -i owasp/zap2docker-stable zap.sh -daemon -host 0.0.0.0 -port 8080 -config api.addrs.addr.name=.* -config api.addrs.addr.regex=true -config api.key=<api-key>
  - To run a baseline scan against any example website use the following command:
  - docker run -t owasp/zap2docker-weekly zap-baseline.py -t https://www.example.com
  - The baseline scan runs the ZAP spider against the specified target for 1 minute and then waits for the passive scanning to complete before reporting the results
    *For more detailed documentation especially on command flags visit the following URLS*
	
	https://github.com/zaproxy/zaproxy/wiki/Docker
	https://github.com/zaproxy/zaproxy/wiki/ZAP-Baseline-Scan
    https://github.com/zaproxy/zaproxy/wiki/InternalDetails
	https://blog.mozilla.org/security/2017/01/25/setting-a-baseline-for-web-security-controls/
	
	*A good tutorial video talking on ZAP is linked below*
	
	https://www.youtube.com/watch?v=o_JZRgQMF4Q
	
	<h1>Configuring ZAP proxy to trace browser traffic</h1>
	
	<h2>Step 1 - Setting ZAP local proxy</h2>
	
	- Go to Tools => Options => Local Proxies and set the address and port number for the proxy. Example Below:
	
	
	<img src="Images/ZAPproxy.PNG">
	
	
	- In this example we used localhost as the address and port 8080 as the Port number
	
	<h2>Step 2 - Configure Proxy in Firefox</h2>
	
	- Go to Options => Network Proxy Settings 
	
	
	<img src="Images/FirefoxSettings.PNG">
	
	
	- Select Manual proxy configuration and enter in the ZAP address and port number, Example Below:
	
	
	<img src="Images/FirefoxProxy.PNG">
	

	
	
	<h2>Step 3 - Testing ZAP proxy Configuration</h2>

	
	- Once the Proxy has been set properly, navigate to a website, you will start noticing the HTTP response and Request under the Sites tab in ZAP
	
	
	<img src="Images/ZAPResponse.PNG">
	

	
	Downloading and installing Selenium WebDriver

	<h1>Downloading and installing Selenium WebDriver</h1>

	
	- Follow this detailed documentation on how to set up Selenium using Java
	
	https://www.guru99.com/installing-selenium-webdriver.html
	
	
	
	

	
	
	
 
    
	
  	
	
 
 
 
 
 
 - 



 
 
 
 

