#!/bin/bash

# Colors for output
blue='\033[1;34m'
green='\033[1;32m'
red='\033[1;31m'
reset='\033[0m'

# Banner
echo -e "${blue}==========================================="
echo -e "   Professional Termux Desktop Environment"
echo -e "===========================================${reset}"

# Update and Upgrade
echo -e "${green}[+] Updating and upgrading Termux packages...${reset}"
pkg update -y && pkg upgrade -y

# Install XFCE Desktop Environment
echo -e "${green}[+] Installing XFCE desktop environment...${reset}"
pkg install x11-repo -y
pkg install xfce4 xfce4-terminal -y

# Install VNC Server
echo -e "${green}[+] Installing TigerVNC server...${reset}"
pkg install tigervnc -y

# Install Firefox Browser
echo -e "${green}[+] Installing Firefox browser...${reset}"
pkg install firefox -y

# Configure VNC Server
echo -e "${green}[+] Configuring VNC server...${reset}"
mkdir -p ~/.vnc
echo "#!/bin/bash
xrdb $HOME/.Xresources
startxfce4 &" > ~/.vnc/xstartup
chmod +x ~/.vnc/xstartup

# Create VNC start/stop scripts
echo "vncserver -geometry 1280x720" > start-desktop.sh
echo "vncserver -kill :1" > stop-desktop.sh
chmod +x start-desktop.sh stop-desktop.sh

# Add Hacker-Style Customizations
echo -e "${green}[+] Adding hacker-style blue and green wallpaper...${reset}"
mkdir -p ~/Desktop/Wallpapers
curl -o ~/Desktop/Wallpapers/hacker-wallpaper.jpg "https://via.placeholder.com/1280x720.png?text=Hacker+Pro+Environment"

echo -e "${green}[+] Setting up hacker theme...${reset}"
xfconf-query -c xfce4-desktop -p /backdrop/screen0/monitor0/workspace0/last-image -s ~/Desktop/Wallpapers/hacker-wallpaper.jpg

# Install cmatrix for hacker-style terminal
echo -e "${green}[+] Installing CMatrix for hacker display...${reset}"
pkg install cmatrix -y

# Display Finish Message
echo -e "${blue}==========================================="
echo -e "${green}Installation Complete!${reset}"
echo -e "${green}Use the following commands to start your desktop:${reset}"
echo -e "${blue}1. ./start-desktop.sh${reset} - Start the desktop environment"
echo -e "${blue}2. ./stop-desktop.sh${reset} - Stop the desktop environment"
echo -e "${blue}3. Connect to the desktop using a VNC viewer at localhost:5901${reset}"
echo -e "${blue}4. Run 'cmatrix' in the terminal for a hacker-style effect.${reset}"

# End
exit 0
