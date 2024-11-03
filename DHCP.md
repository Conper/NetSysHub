# DHCP Server in Docker

This project sets up a DHCP server using Docker, allowing you to quickly and isolatedly assign IP addresses on your local network.

![DockerDHCP](https://github.com/user-attachments/assets/35df83bf-bbb4-4099-8990-4ce1208b8df0)


## Requirements

- **Docker** installed on your system.
- Access to your computer's network interface (using `host` mode is recommended for Docker to access the network directly).

## Configuration Steps

### 1. Prepare the configuration file `dhcpd.conf`

Before starting, we’ll create a folder named `data` to store the necessary data that the container will use during its execution.

The `dhcpd.conf` file defines the IP range, gateway, DNS, and other network parameters that the DHCP server will provide to clients. Make sure to customize this file before starting the server. We will save it inside the `data` folder.

Example of a basic `dhcpd.conf` file:

```js
default-lease-time 600;
max-lease-time 7200;

subnet 192.168.1.0 netmask 255.255.255.0 {
    range 192.168.1.50 192.168.1.100;
    option routers 192.168.1.1;
    option domain-name-servers 8.8.8.8, 8.8.4.4;
}
```

### 2. Download the Docker image for DHCP

We will use the `networkboot/dhcpd` image, which includes the DHCP service ready to be configured and run.

```
docker pull networkboot/dhcpd
```

### 3. Run the Docker container

Start the container using the **`docker run`** command. Make sure to mount the **`dhcpd.conf`** configuration file in the container and use the **`host`** network so that the DHCP service can communicate directly with your local network.

```js
docker run --name dhcp-server \
    -v "/path/data/dhcpd.conf:/etc/dhcp/dhcpd.conf" \
    -v "/path/data:/data" \
    --network host \
    networkboot/dhcpd eth0
```

- `--name dhcp-server`: Assigns a name to the container.
- `-v /path/dhcpd.conf:/etc/dhcp/dhcpd.conf`: Mounts the local configuration file in the container.
- `--network host`: Allows the container to access the network directly.
- `networkboot/dhcpd eth0`: Uses the `networkboot/dhcpd` image and specifies the network interface (`eth0`).

### 4. Verify the DHCP server

To verify that the DHCP server is running, you can check the container logs:

```
docker logs dhcp-server
```

Here you will see the IP assignments and any errors that occur when starting the DHCP server.

## Stop and Restart the Server

- **Stop the server**:
  ```
  docker stop dhcp-server
  ```

- **Restart the server**:
  ```bash
  docker restart dhcp-server
  ```

> [!NOTE]
>
> - This DHCP server in Docker will only work correctly in environments that support `host` networking, as DHCP requires direct communication with the local network.
> - Make sure to adapt the `dhcpd.conf` file to the specific requirements of your network.
