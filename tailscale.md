# 1. Update package lists while ignoring broken experimental repositories   
apt update --allow-unauthenticated || true  
  
# 2. Add the official Tailscale repository to your system configuration  
curl -fsSL https://tailscale.com | tee /usr/share/keyrings/tailscale-archive-keyring.gpg >/dev/null  
curl -fsSL https://tailscale.com | tee /etc/apt/sources.list.d/tailscale.list  
  
# 3. Refresh lists to include Tailscale, skipping any broken repository checks  
apt update --allow-unauthenticated || true  
  
# 4. Force install Tailscale while skipping missing package errors  
apt install tailscale -y --fix-missing  
  
# 5. Spin up Tailscale and link your account  
tailscale up  

echo 'net.ipv4.ip_forward = 1' \| tee -a /etc/sysctl.d/99-tailscale.sysctl
sysctl -p /etc/sysctl.d/99-tailscale.sysctl
tailscale up --advertise-routes=192.168.100.0/24
