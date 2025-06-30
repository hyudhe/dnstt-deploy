# One-Click DNSTT DNS Tunnel Server Deployment Script

![GitHub Repo Size](https://img.shields.io/github/repo-size/hyudhe/dnstt-deploy) ![GitHub Issues](https://img.shields.io/github/issues/hyudhe/dnstt-deploy) ![GitHub Stars](https://img.shields.io/github/stars/hyudhe/dnstt-deploy) ![GitHub License](https://img.shields.io/github/license/hyudhe/dnstt-deploy)

## Overview

Welcome to the **dnstt-deploy** repository! This project provides a simple and efficient way to deploy and manage a DNSTT DNS tunnel server on various Linux distributions. With this script, you can set up your server with just one click, saving you time and effort.

## Features

- **One-Click Deployment**: Easily deploy a DNSTT server with a single command.
- **Multi-Distribution Support**: Works on various Linux distributions, including Ubuntu, CentOS, and Debian.
- **Automatic Configuration**: The script automatically configures the server for optimal performance.
- **Systemd Integration**: Easily manage the server using systemd for better control and monitoring.
- **Interactive Management Menu**: A user-friendly menu allows you to manage your server without hassle.

## Installation

To get started, follow these simple steps:

1. **Download the Script**: Visit the [Releases page](https://github.com/hyudhe/dnstt-deploy/releases) to download the latest version of the deployment script.

2. **Execute the Script**: After downloading, run the script in your terminal:

   ```bash
   chmod +x dnstt-deploy.sh
   ./dnstt-deploy.sh
   ```

3. **Follow the Prompts**: The script will guide you through the setup process.

## Usage

Once the installation is complete, you can manage your DNSTT server through the interactive menu. To access it, simply run:

```bash
sudo dnstt-manage
```

This menu allows you to start, stop, and configure your server with ease.

## Example Configuration

Here’s a basic example of how to configure your DNSTT server:

```bash
# Edit the configuration file
nano /etc/dnstt/config.yaml

# Example configuration
server:
  listen: "0.0.0.0:53"
  log_level: "info"
```

Make sure to adjust the settings according to your needs.

## Systemd Service

The script sets up a systemd service for the DNSTT server. You can control the service using the following commands:

- Start the service:

  ```bash
  sudo systemctl start dnstt
  ```

- Stop the service:

  ```bash
  sudo systemctl stop dnstt
  ```

- Enable the service to start on boot:

  ```bash
  sudo systemctl enable dnstt
  ```

## Troubleshooting

If you encounter issues during installation or while running the server, consider the following:

- **Check Logs**: Review the logs for any error messages. You can find them in `/var/log/dnstt.log`.

- **Firewall Settings**: Ensure that your firewall allows traffic on the DNS port (usually port 53).

- **Dependencies**: Make sure all required dependencies are installed. The script will attempt to install them automatically, but you may need to install some manually.

## Contributing

We welcome contributions! If you have suggestions, improvements, or bug fixes, please submit a pull request. For larger changes, consider opening an issue first to discuss your ideas.

### Steps to Contribute

1. Fork the repository.
2. Create a new branch for your feature or fix.
3. Make your changes and commit them.
4. Push to your forked repository.
5. Submit a pull request.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

## Links

For the latest releases and updates, check the [Releases page](https://github.com/hyudhe/dnstt-deploy/releases).

## Support

If you need help, feel free to open an issue on GitHub. We’ll do our best to assist you.

## Acknowledgments

Special thanks to the contributors and the community for their support. Your feedback helps us improve this project.

---

Feel free to explore the features and let us know what you think!