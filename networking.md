

### Part 1: Check Current IP Configuration and Configure a Static IP Address (10 Minutes)

1. **Open Terminal**
   - Launch a terminal (Ctrl + Alt + T).

2. **Check Current IP Configuration**
   - Run the following command to check the current IP addresses assigned to your network interfaces:
     ```bash
     ip addr show
     ```
   - Identify the interface you want to configure (e.g., `eth0`, `ens33`, etc.) and note its current IP address, subnet mask, and any existing configurations.

3. **Select a Static IP Address**
   - Choose an unused static IP address from the same subnet as your current configuration. For example, if your current configuration shows `192.168.1.5/24`, you might choose `192.168.1.100/24` as your static IP.
   - Ensure that this IP is outside the DHCP range of your router to avoid conflicts.

4. **Edit the Netplan Configuration**
   - Open the Netplan configuration file for editing. You might need to check which file exists in `/etc/netplan/`:
     ```bash
     ls /etc/netplan/
     ```
   - Edit the relevant YAML file (replace `01-netcfg.yaml` with the actual filename if different):
     ```bash
     sudo nano /etc/netplan/01-netcfg.yaml
     ```
   - Update the configuration to set the selected static IP address. Use the following template, replacing `eth0` and the addresses as needed:
     ```yaml
     network:
       version: 2
       renderer: networkd
       ethernets:
         eth0:
           dhcp4: no
           addresses:
             - 192.168.1.100/24  # Replace with your chosen static IP
           gateway4: 192.168.1.1  # Your router's IP
           nameservers:
             addresses:
               - 8.8.8.8          # Google's DNS
               - 8.8.4.4
     ```
   - Save the changes and exit the editor (Ctrl + O, Enter to save, and Ctrl + X to exit).

5. **Apply the Configuration**
   - Run the following command to apply the new network configuration:
     ```bash
     sudo netplan apply
     ```
   - To verify that the configuration was applied, check the interface again:
     ```bash
     ip addr show eth0
     ```

### Part 2: Set Up Routing and NAT (10 Minutes)

1. **Enable IP Forwarding**
   - Temporarily enable IP forwarding with:
     ```bash
     echo 1 | sudo tee /proc/sys/net/ipv4/ip_forward
     ```
   - Make this change permanent by editing the sysctl configuration:
     ```bash
     sudo nano /etc/sysctl.conf
     ```
   - Uncomment or add the line:
     ```plaintext
     net.ipv4.ip_forward=1
     ```
   - Apply the changes immediately:
     ```bash
     sudo sysctl -p
     ```

2. **Configure NAT with iptables**
   - To allow devices on your local network to access the internet via your Ubuntu machine, set up NAT:
     ```bash
     sudo iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
     ```
   - This command assumes that `eth0` is your external interface. If your external interface has a different name, adjust accordingly.

3. **Save iptables Rules**
   - Install the `iptables-persistent` package to save your NAT rules:
     ```bash
     sudo apt install iptables-persistent
     ```
   - During installation, you’ll be prompted to save current rules. Select "Yes."

### Part 3: Troubleshooting (10 Minutes)

1. **Check Connectivity**
   - Use `ping` to check connectivity to your gateway:
     ```bash
     ping -c 4 192.168.1.1
     ```
   - If you receive replies, your gateway is reachable. If not, check your physical connections or verify your IP configuration.

2. **Test Internet Connectivity**
   - Test your internet connectivity by pinging a public IP (e.g., Google's DNS):
     ```bash
     ping -c 4 8.8.8.8
     ```
   - If this works, try pinging a domain name to check DNS resolution:
     ```bash
     ping -c 4 google.com
     ```
   - If the IP ping works but the domain fails, check your DNS settings in the `/etc/resolv.conf` file:
     ```bash
     cat /etc/resolv.conf
     ```

3. **Check the Routing Table**
   - Display the routing table to ensure your routes are correctly set:
     ```bash
     ip route show
     ```
   - You should see a default route pointing to your gateway (e.g., `default via 192.168.1.1`).

4. **Review Logs**
   - Check system logs for networking-related messages, which can help diagnose issues:
     ```bash
     tail -f /var/log/syslog
     ```

5. **Use Additional Commands for Diagnosis**
   - Check if the network interface is up:
     ```bash
     ip link show eth0
     ```
   - If it’s down, bring it up:
     ```bash
     sudo ip link set eth0 up
     ```

### Deliverables
- **Submit a report** that includes:
  - The output of your IP configuration (`ip addr show`).
  - The output of your routing table (`ip route show`).
  - A summary of any troubleshooting steps taken and their results.
