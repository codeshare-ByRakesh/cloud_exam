# Create a Windows EC2 Instance and Host a simple Web Page

## Objective
- To create a Windows EC2 instance, install Internet Information Services (IIS), and host a website that displays a welcome message.


#### Step 1: Launch the Windows EC2 Instance
- *Go to the AWS Management Console and navigate to EC2.*
- Click Launch Instance and choose a Windows Server (e.g., Windows Server 2019).
- Select an instance type (e.g., t2.micro).
- Configure security groups to allow:
- RDP (port 3389)
- HTTP (port 80)
- *Launch the instance and download the .pem key file if needed.*
  
#### Step 2: Connect to Your Windows EC2 Instance
- In the EC2 Dashboard, select your Windows instance and click Connect.
- Choose RDP Client and download the Remote Desktop file.
- Decrypt the Administrator password using your .pem key.
- Open the RDP file and enter the credentials to access your instance.


#### Step 3: Set Up IIS Web Server
- Open Server Manager and go to Add roles and features.
- Install IIS (Internet Information Services) to set up the web server.

#### Step 4: Create the Web Page
- Open IIS Manager and navigate to Default Web Site.
- Explore Folder:
- Go to   ```bash C:\inetpub\wwwroot and create index.html. ```
- Add the following HTML:

```bash
<html>
  <body>
    <h1>Welcome to HPCSA....Suraj Kumar</h1>
  </body>
</html>
```
- Save and close.
  
#### Step 5: Verify the Website
- Open Browser and go to http://<public_ip_of_your_instance> to see the "Welcome to HPCSA...Suraj Kumar" message.


<br>
<br>

#  Transfer Files from local machine to Windows EC2 Instance.
## File Transfer:

- In your RDP session, use copy-paste or drag-and-drop to move files from your local machine to the Windows desktop or another folder.


<br>
<br>



## Set Up  Web Server using  CMD Terminal


#### Step 1: Install IIS

- Open Command Prompt as Administrator:

- Install IIS:
```bash
dism /online /enable-feature /featurename:IIS-WebServer /all
```
- Verify IIS Installation:
```bash
dism /online /get-features | findstr /i "IIS-WebServer"
```

#### Step 2: Create the Website Directory
- Check if the directory exists:
```bash
dir C:\inetpub
```
- If wwwroot does not appear, create it:
```bash
mkdir C:\inetpub\wwwroot
```

Step 3: Create the HTML File
- Open Notepad:
```bash
notepad C:\inetpub\wwwroot\index.html
```
- Enter the HTML content:
  
```bash
<html>
<body>
    <h1>Welcome to HPCSA... Suraj Kumar!!!</h1>
</body>
</html>
```
- Save the file and close Notepad.

#### Step 4: Start IIS
Restart IIS (if necessary):
```bash
net stop w3svc
net start w3svc
```
#### Step 5: Test the Website
- Access your website by opening a web browser and navigating to:
```bash
http://<your-instance-public-ip>
```
## You should see the message: "Welcome to HPCSA... Suraj Kumar!!!".

<br>
<br>

## Copy Files from Local Machine to Windows Server
Simply drag and drop or copy-paste files from your local machine to the Windows EC2 instance desktop or other desired folder using the RDP session.
Troubleshooting
- If you encounter issues with creating files in the wwwroot directory, ensure that you have the necessary permissions by checking the Security tab in the folder properties.





