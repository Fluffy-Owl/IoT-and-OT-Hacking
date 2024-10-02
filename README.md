# IoT-and-OT-Hacking

The labs will provide you with real-time experience in performing _footprinting_ and _analyzing traffic_ between IoT and OT devices.

---
# Perform footprinting using various footprinting techniques

**1. Gather Information using Online Footprinting Tools**

In this task, we will focus on performing footprinting on the MQTT protocol, which is a machine-to-machine (M2M)/“Internet of Things” connectivity protocol. It is useful for connections with remote locations where a small code footprint is required and/or network bandwidth is at a premium.

1. https://www.whois.com/whois/
2. type www.oasis-open.org [Oasis is an organization that has published the MQTT v5.0 standard, which represents a significant leap in the refinement and capability of the messaging protocol that already powers IoT.]
3. The result appears, displaying the following information, as shown in the screenshots: **Domain Information, Registrant Contact, and Raw Whois Data**.
![image](https://github.com/user-attachments/assets/c25a2244-3739-48e5-887b-a1890cbefd56)
![image](https://github.com/user-attachments/assets/76a9101b-9abf-4be5-a551-9f9197b58f80)

1. https://www.exploit-db.com/google-hacking-database
2. The Google Hacking Database page appears; type SCADA in the Quick Search field and press Enter.
3. The result appears, which displays the Google dork related to SCADA, as shown in the screenshot.
![image](https://github.com/user-attachments/assets/b96266bd-4d0a-47ae-a035-0c3ea1924a3a)

4. Now, we will use the dorks obtained in the previous step to query results in Google.
5. Open a new tab and type https://www.google.com in the address bar, and press Enter.
6. In the search field, type `"login" intitle:"scada login"` and click the Google Search button.
7. The search result appears; click any link (here, SCADA :: seamtec SCADA login ::).
8. The seamtec SCADA login page appears, as shown in the screenshot.
9. Similarly, you can use advanced search operators such as intitle:"index of" scada to search sensitive SCADA directories that are exposed on sites.

1. https://account.shodan.io/login
2. The Login with Shodan page appears; enter your username and password in the Username and Password fields, respectively; and click Login. Username=Flfuffyowl
3. The Shodan main page appears; type port:1883 in the address bar and press Enter.
4. The result appears, displaying the list of IP addresses having port 1883 enabled, as shown in the screenshot.
5. Click on any IP address to view its detailed information.
6. Detailed results for the selected IP address appears, displaying information regarding Ports, Services, Hostnames, ASN, etc.
7. Similarly, you can gather additional information on a target device using the following Shodan filters:

Search for Modbus-enabled ICS/SCADA systems:

port:502

Search for SCADA systems using PLC name:

“Schneider Electric”

Search for SCADA systems using geolocation:

SCADA Country:"US"


---
# Capture and Analyze IoT Device Traffic

**1. Capture and Analyze IoT Traffic using Wireshark**


1. To install the MQTT Broker on the Windows Server 2019, click Windows Server 2019 to launch Windows Server 2019 machine.
2. Navigate to Z:\CEH-Tools\CEHv12 Module 18 IoT and OT Hacking\Bevywise IoT Simulator folder and double-click on the Bevywise_MQTTRoute_Win_64.exe file.
3. The installation completes, click on Finish. Ensure that Launch Bevywise_MQTTRoute_Win_64 is checked.
4. The MQTTRoute will execute and the command prompt will appear. You can see the TCP port using 1883.
5. We have installed MQTT Broker successfully and leave the Bevywise MQTT running.
6. To create IoT devices, we must install the IoT simulator on the client machine.
7. Open another server 2022
8. Navigate to Z:\CEH-Tools\CEHv12 Module 18 IoT and OT Hacking\Bevywise IoT Simulator folder and double-click on the Bevywise_IoTSimulator_Win_64.exe file.
9. Bevywise IoT Simulator is installed successfully. To launch the IoT simulator, navigate to the C:\Bevywise\IotSimulator\bin directory and double-click on the runsimulator.bat file.
10. Upon double-clicking the runsimulator.bat file opens in the command prompt. If How do you want to open this? pop-up appears, select **Microsoft Edge browser** and click OK to open the URL http://127.0.0.1:9000/setnetwork?network=HEALTH_CARE.
11. The web interface of the IoT Simulator opens in Edge browser. In the IoT Simulator, you can view the default network named **HEALTH_CARE** and several devices.
12. Next, we will create a virtual IoT network and virtual IoT devices. Click on the menu icon and select the +New Network option.
![image](https://github.com/user-attachments/assets/64dd7f5b-3ec7-4e51-8d97-e77f068c6439)

13. The Create New Network popup appears. Type any name (here, **CEH_FINANCE_NETWORK**) and description. Click on Create.
14. In the next screen, we will setup the **Simulator Settings**. Set the **Broker IP Address as 10.10.1.19** (the IP address of the Windows Server 2019 ). Since we have installed the Broker on the web server, the created network will interact with the server using MQTT Broker. Do not change default settings and click on Save.
15. To add IoT devices to the created network, click on the Add blank Device button.
16. The Create New Device popup opens. Type the device name (here, we use Temperature_Sensor), enter Device Id (here, we use TS1), provide a Description and click on Save.
17. The device will be added to the CEH_FINANCE_NETWORK.
18. To connect the Network and the added devices to the server or Broker, click on the Start Network red color circular icon in right corner.
19. When a connection is established between the network and the added devices and the web server or the MQTT Broker, the red button turns into green.
20. Next, switch to the Windows Server 2019 machine. Since the Broker was left running, you can see a connection request from machine 10.10.1.22 for the device TS1.
![image](https://github.com/user-attachments/assets/75abbdbd-7c05-4d5f-9408-3d6db2f3c9f4)

21. Switch back to Windows Server 2022 machine.
22. Next, we will create the Subscribe command for the device Temperature_Sensor.
23. Click on the Plus icon in the top right corner and select the Subscribe to Command option.
24. The Subscribe for command - TS1 popup opens. Select **On start** under the Subscribe on tab, type **High_Tempe** under the Topic tab, and select **1 Atleast once** below the Qos option. Click on Save.
25. Scroll down the page, you can see the Topic added under the Subscribe to Commands section.
26. Next, we will capture the traffic between the virtual IoT network and the MQTT Broker to monitor the secure communication.
  

1. Minimise the Edge browser. Click on Type here to search at the bottom left of the desktop, type wireshark and select Wireshark from the results to launch the Wireshark from the application list.
2. The Wireshark Application window appears, select the Ethernet as interface.
3. Click on the Start Wireshark icon to start the capturing packets, leave the Wireshark running.
4. Leave the IoT simulator running and switch to the Windows Server 2019 machine.
5. Minimise all opened applications and windows, Open Chrome browser, type http://localhost:8080 and press Enter.
6. As soon as you press Enter, the MQTTRoute Sign in page appears, keep the default credential unchanged and click on SIGN IN.
7. Navigate to Devices menu. You will be able to see the connected device TS1 in the left pane.
8. Now, we will send the command to TS1 using the High_Tempe topic.
9. Go to the Command Send section, select Topic as High_Tempe, type Alert for High Temperature and click on the Send button.
![image](https://github.com/user-attachments/assets/09bb4cea-dfa9-42f5-80b0-f9e3747aedd2)

10. The alert popup appears, then click on OK.
11. The message has been sent to the device using this topic.
12. Next, switch to Windows Server 2022 machine.
13. We have left the IoT simulator running in the web browser. To see the alert message, maximise the Edge browser and expand the arrow under the connected Temperature_Sensor, Device Log section.
12. You can see the alert message "Alert for High Temperature"
13. To verify the communication, we have executed Wireshark application, switch to the Wireshark traffic capturing window.
14. Type mqtt under the filter field and press Enter. To display only the MQTT protocol packets.
15. Select any Publish Message packet from the Packet List pane. In the Packet Details pane at the middle of the window, expand the Transmission Control Protocol, MQ Telemetry Transport Protocol, and Header Flags nodes.
16. Under the MQ Telemetry Transport Protocol nodes, you can observe details such as Msg Len, Topic Length, Topic, and Message.
17. Publish Message can be used to obtain the message sent by the MQTT client to the broker.
![image](https://github.com/user-attachments/assets/0296679e-3ab1-45cb-b396-02c4e113b1f5)

18. Select any Publish Release packet from the Packet List pane. In the Packet Details pane at the middle of the window, expand the Transmission Control Protocol, MQ Telemetry Transport Protocol, and Header Flags nodes.
19. Under the MQ Telemetry Transport Protocol nodes, you can observe details such as Msg Len, Message Type, Message Identifier.

20. Now, scroll down, look for the Publish Complete packet from the Packet List pane, and click on it. In the Packet Details pane at the middle of the window, expand the Transmission Control Protocol, MQ Telemetry Transport Protocol, and Header Flags nodes.
21. Under the MQ Telemetry Transport Protocol nodes, you can observe details such as Msg Len and Message Identifier.

22. Now, scroll down, look for the Publish Received packet from the Packet List pane, and click on it. In the Packet Details pane at the middle of the window, expand the Transmission Control Protocol, MQ Telemetry Transport Protocol, and Header Flags nodes.
23. Under the MQ Telemetry Transport Protocol nodes, you can observe details such as Message Type, Msg Len and Message Identifier.

24. Similarly you can select Ping Request, Ping Response and Publish Ack packets and observe the details.


This concludes the demonstration of capturing and analyzing MQTT protocol packets. Here, we analyzed different processes involved in the communication between an MQTT client and an MQTT broker using Wireshark. Understanding these metrics as well as the workflow can help you in quickly identifying the MQTT-related issues.















