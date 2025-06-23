# dnstt-deploy

🚀 **One-click dnstt server deployment and management**

A comprehensive automation script for deploying and managing [dnstt](https://www.bamsoftware.com/software/dnstt) DNS tunnel servers on Linux systems. This script handles everything from installation to configuration, making DNS tunnel deployment effortless.

## DNS Domain Setup

Before using this script, you need to properly configure your domain's DNS records. Here's the required setup:

### Example Configuration
- **Your domain name**: `example.com`
- **Your server's IPv4 address**: `203.0.113.2`
- **Your server's IPv6 address**: `2001:db8::2` (optional)
- **Tunnel subdomain**: `t.example.com`
- **Server hostname**: `tns.example.com`

### DNS Records Setup
Go into your name registrar's configuration panel and add these records:

| Type | Name | Points to |
|------|------|-----------|
| A | `tns.example.com` | `203.0.113.2` |
| AAAA | `tns.example.com` | `2001:db8::2` (if IPv6 available) |
| NS | `t.example.com` | `tns.example.com` |

**Important**: Wait for DNS propagation (can take up to 24 hours) before testing your tunnel.

## Features

- **Multi-distribution support**: Fedora, Rocky Linux, CentOS, Debian, Ubuntu
- **Automatic detection**: OS, architecture, and SSH port detection
- **Security hardened**: Non-root service execution with systemd security features
- **Smart configuration**: Persistent settings and automatic key reuse
- **Flexible tunneling**: SSH mode or SOCKS proxy mode
- **Network ready**: Automatic firewall and iptables configuration
- **Binary verification**: MD5, SHA1, and SHA256 checksum validation
- **Official binaries**: Downloads from [dnstt.network](https://dnstt.network)

## Quick Start

### Prerequisites
- Linux server (Fedora, Rocky, CentOS, Debian, or Ubuntu)
- Root access or sudo privileges
- Internet connection for package downloads
- **Domain name with proper DNS configuration** (see DNS Domain Setup section above)

### Installation

1. **Download the script:**
   ```bash
   wget https://raw.githubusercontent.com/bugfloyd/dnstt-deploy/main/dnstt-deploy.sh
   chmod +x dnstt-deploy.sh
   ```

2. **Run the deployment:**
   ```bash
   sudo ./dnstt-deploy.sh
   ```

3. **Follow the interactive prompts:**
    - Enter your nameserver subdomain (e.g., `t.example.com`)
    - Set MTU value (default: 1232)
    - Choose tunnel mode (SSH or SOCKS)

## Configuration

### Tunnel Modes

**SSH Mode (Option 2)**
- Tunnels DNS traffic to your SSH service
- Automatically detects SSH port (default: 22)
- Perfect for secure shell access via DNS
- Compatible with mobile apps

**SOCKS Mode (Option 1)**
- Sets up integrated Dante SOCKS5 proxy
- Listens on `127.0.0.1:1080`
- Provides full internet proxy capabilities

### MTU Settings
- **Default**: 1232 bytes
- **Range**: 512-1400 bytes
- **Recommended values**:
    - Stable/Fast networks: 1400
    - Standard networks: 1232
    - Unstable/Slow networks: 1200
    - Restricted mobile networks: 512

### Changing Settings
To change MTU or other settings, simply re-run the script and enter new values. The script will automatically update the configuration and restart services.

## Client Usage

### Download Client Binary

Download the appropriate dnstt client binary for your platform from [dnstt.network](https://dnstt.network):

**Common platforms:**
- **Linux x64**: `dnstt-client-linux-amd64`
- **Windows x64**: `dnstt-client-windows-amd64.exe`
- **macOS Intel**: `dnstt-client-darwin-amd64`
- **macOS Apple Silicon**: `dnstt-client-darwin-arm64`

### Connection Command

After server deployment, you'll receive a public key. Use it to connect:

```bash
dnstt-client -udp DNS_SERVER_IP:53 -pubkey-file server.pub t.example.com 127.0.0.1:7000
```

#### DNS_SERVER_IP

The DNS server IP depends on your system:

**Linux**:
```bash
systemd-resolve --status | grep "DNS Servers"
```
Common options: `127.0.0.53:53`, `127.0.0.1:53`, or your router's IP

**Windows**:
```cmd
ipconfig /all | findstr /C:"DNS Servers"
```

**macOS**:
```bash
scutil --dns | grep nameserver
```

You can also use:
- Your router/modem internal IP (e.g., `192.168.1.1:53`)
- Your ISP's DNS server IP
- Public DNS servers like `8.8.8.8:53` or `1.1.1.1:53` (If Available)

### Mobile Apps (SSH Mode)

For SSH tunnels, you can use these Android/iOS apps without needing a computer:

**Android**:
- [HTTP Injector](https://play.google.com/store/apps/details?id=com.evozi.injector)
- [HTTP Custom](https://play.google.com/store/apps/details?id=xyz.easypro.httpcustom)
- [DarkTunnel](https://play.google.com/store/apps/details?id=net.darktunnel.app)

**iOS**:
- [HTTP Injector](https://apps.apple.com/us/app/http-injector/id1659992827)

## Management

### File Locations

```
/usr/local/bin/dnstt-server          # Main binary
/etc/dnstt/                          # Configuration directory
├── dnstt-server.conf               # Main configuration
├── {domain}_server.key             # Private key (per domain)
└── {domain}_server.pub             # Public key (per domain)
/etc/systemd/system/dnstt-server.service  # Systemd service
```

### Service Commands

**dnstt-server Service**:
```bash
sudo systemctl status dnstt-server    # Check status
sudo systemctl start dnstt-server     # Start service
sudo systemctl stop dnstt-server      # Stop service
sudo systemctl restart dnstt-server   # Restart service
sudo journalctl -u dnstt-server -f    # View logs
```

**Dante SOCKS Service (SOCKS mode only)**:
```bash
sudo systemctl status danted          # Check status
sudo systemctl start danted           # Start service
sudo systemctl stop danted            # Stop service
sudo systemctl restart danted         # Restart service
sudo journalctl -u danted -f          # View logs
```

## Troubleshooting

### MTU Size Errors

If you see errors like this:
```
FORMERR: requester payload size 512 is too small (minimum 1232)
```

Lower the MTU to the mentioned number by re-running the script and entering the suggested MTU value. Consider the performance trade-offs when using very low MTU values.

### Common Issues

**Service Won't Start**:
```bash
sudo systemctl status dnstt-server    # Check service status
sudo journalctl -u dnstt-server -n 50 # Check logs for errors
ls -la /usr/local/bin/dnstt-server    # Verify binary permissions
```

**DNS Configuration Issues**:
```bash
dig @YOUR_SERVER_IP t.mydomain.com           # Test DNS tunnel (from client)
sudo iptables -t nat -L PREROUTING -n -v     # Check iptables rules
```

**SOCKS Proxy Issues**:
```bash
curl --socks5 127.0.0.1:1080 http://httpbin.org/ip  # Test SOCKS proxy locally
sudo cat /etc/danted.conf                           # Check Dante configuration
```

**Port Check**:
```bash
sudo ss -tulnp | grep 5300  # Check dnstt-server port
sudo ss -tulnp | grep 1080  # Check SOCKS proxy port (if enabled)
```

## Advanced Features

### Multiple Domain Support

The script supports multiple domains by generating separate key pairs for each domain. However, only one domain configuration can be active at a time since all configurations use port 53 for DNS traffic. To switch between domains, simply re-run the script with a different subdomain.

### DoH and DoT Support

For DNS-over-HTTPS (DoH) or DNS-over-TLS (DoT) configurations, refer to the [official dnstt documentation](https://dnstt.network) for detailed setup instructions.

### Performance Monitoring

```bash
sudo ss -tulnp | grep -E "(5300|1080)"        # Monitor connection count
sudo systemctl status dnstt-server            # Check service resources
sudo journalctl -u dnstt-server -f --no-pager # Monitor logs for errors
```

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request. For major changes, please open an issue first to discuss what you would like to change.

## Acknowledgments

- [dnstt](https://dnstt.network) by David Fifield
- [Dante SOCKS server](https://www.inet.no/dante/) for SOCKS proxy functionality

## Support

- **Issues**: [GitHub Issues](https://github.com/bugfloyd/dnstt-deploy/issues)
- **Discussions**: [GitHub Discussions](https://github.com/bugfloyd/dnstt-deploy/discussions)
- **Official project website**: [dnstt.network](https://dnstt.network)

---

**Made with ❤️ for privacy and security**