# <！-- by 黄朝淼 -->🇬🇧 English Version README.md
# Set Up a Custom Supernode
## Overview
This guide will provide a detailed introduction on how to set up a custom supernode, and it will also cover relevant code explanations. By setting up a custom supernode, you can create your own n2n network infrastructure, enhancing network security and privacy.

## I. Operation Steps for Setting a Custom Supernode
### Installing the n2n Software Package
Install the n2n software package on a public server (such as a VPS). The installation methods vary depending on the operating system as follows:

1.For Ubuntu/Debian systems: Open the terminal and execute the command sudo apt-get update && sudo apt-get install n2n to update the software source and install n2n.
2.For CentOS systems: First, ensure that the EPEL source is installed. Then, execute sudo yum install n2n in the terminal to install the n2n software package.
### Editing the Configuration File
Use a text editor (such as nano or vim) to open the /etc/n2n/supernode.conf file and add the content -p=1234. Here, 1234 is the specified port, which can be modified according to the actual situation. This operation is to set the port that the supernode listens on.
### Starting the supernode Service
Enter the command sudo systemctl start supernode in the terminal to start the supernode service, allowing the supernode to run on the set port.
### Setting Auto-start on Boot (Optional)
If you want the supernode service to start automatically after the server restarts, execute the command sudo systemctl enable supernode in the terminal.
## Parameter Processing Logic and Initialization Logic
The setOption function is the core logic for handling command-line parameters in the n2n supernode. It is responsible for parsing various configuration options and setting the corresponding structure members. The following is a detailed textual explanation of this function:
## Parameter Parsing Process
### 1.Receiving parameters:
 The function receives three parameters: optkey (parameter identifier), _optarg (parameter value), and sss (supernode structure pointer).
### 2.Branch processing:
 Use a switch statement to handle different parameter options according to optkey.
### 3.Port parameter (-p):
Format judgment: First, check whether the parameter value contains a colon : to distinguish different input formats.
IP: port format: If it contains a colon, split the string into an IP address part and a port part. Use inet_addr to convert the IP string into an integer in network byte order, and then convert it into host byte order through ntohl. Use atoi to convert the port string into an integer.
#### Error handling:
If the IP address is invalid (returns INADDR_NONE), it will default to bind to all available interfaces (INADDR_ANY).
If the port number is invalid (parsed as 0), the default port (usually 7777) will be used.
#### Only IP or only port:
If the parameter value contains a dot ., it will be regarded as only specifying the IP address, and the default value will be used for the port.
Otherwise, it will be regarded as only specifying the port number, and the IP address will be bound to all interfaces.
## Security Features
#### 1.Input verification:
 Check the validity of the IP address and port number to prevent the program from crashing due to illegal input.
Default value handling: When the input is invalid, it will automatically fall back to a secure default configuration to ensure that the service can start normally.
#### 2.Log recording:
 Use traceEvent to record the warning information during the parameter parsing process, which is convenient for debugging and maintenance.
## Explanation of Supernode Initialization Logic
The initialization of the supernode is divided into two main functions: sn_init_defaults and sn_init, which complete the initialization work at different stages respectively:
### Basic Initialization (sn_init_defaults)
#### 1.Initializing structure members: 
Set each field of the supernode structure to its default value.
#### 2.Network configuration:
###### 1.bind_address:
 Default to bind to all available interfaces (INADDR_ANY).
###### 2.lport:
 The default listening port (usually 7777).
mport: The management port (used to receive management commands).
###### 3.Memory allocation: 
Allocate initial memory space for dynamic data structures such as the edge node table and the community list.
Setting default parameters: Set other runtime parameters, such as the number of threads and the timeout time.
## Advanced Initialization (sn_init)
###### 1.Thread creation:
Create a resolution thread (resolve_create_thread) to handle domain name resolution and node discovery. This thread is responsible for maintaining the reachability information of edge nodes to ensure smooth communication between nodes.
###### 2.Resource initialization:
Initialize the encryption context and configure the default encryption algorithm.
Start the management interface and listen for management commands.
###### 3.Service preparation:
Create a socket and bind it to the specified address and port.
Start listening for client connection requests.
###### 4.Status notification:
Record the initialization completion information through logs, including key configurations such as the bound address and port.
### Characteristics of the Initialization Process
###### 1.Staged initialization: 
Split the complex initialization process into two stages, basic setting and advanced setting, to improve code maintainability.
###### 2.Modular design:
 Each initialization task is independently encapsulated, making it easy to expand and modify functions.
Resource management: Reasonably allocate and initialize system resources to ensure the normal operation of the service.
###### 3.Error handling:
 When the initialization fails, perform appropriate cleaning and error reporting to ensure the robustness of the program.
### Combined Workflow of the Two
###### 1.Parameter parsing:
 Parse the command-line parameters through the setOption function and modify the default configuration.
###### 2.Basic initialization:
 Call sn_init_defaults to set the default values and build the basic operating environment.
###### 3.Parameter application: 
Apply the parsed parameter values to the initialized structure and overwrite the default values.
###### 4.Advanced initialization:
 Call sn_init to complete the remaining initialization work and start the core service components.
###### 5.Service operation: 
The supernode starts listening for client connections and processes communication requests between nodes.
This design makes the configuration of the supernode flexible and easy to expand, while ensuring the stability and security of the initialization process.

# Edge Node Configuration
## Connect to the Custom Supernode
On your edge node, you can specify the custom supernode using the -l parameter. For example, use the following command on the edge node to connect to the custom supernode:

sudo edge -d tun0 -l your_supernode_ip:1234 -c mycommunity -k mysecretkey -a 192.168.100.2 -f

### Parameter Explanation:
-d tun0: Use the tun0 virtual interface
-l: Specify the supernode address and port
-c: Community name
-k: Encryption key
-a: Assigned virtual IP address
-f: Run in the foreground (for debugging purposes)