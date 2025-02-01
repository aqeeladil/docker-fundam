# Docker Networking

Docker networking allows containers to communicate with each other and with the host system. Containers run in an isolated environment and require networking to enable communication between them.

By default, Docker provides multiple network drivers, including the **bridge**, **host**, **overlay**, and **macvlan** drivers.

You can list available networks using:

```sh
docker network ls
```

Example output:

```
NETWORK ID          NAME                DRIVER
xxxxxxxxxxxx        none                null
xxxxxxxxxxxx        host                host
xxxxxxxxxxxx        bridge              bridge
```

---
## Bridge Networking

Bridge networking is the **default network mode** in Docker. It creates an isolated, private network for containers on a single host, allowing communication between them. Containers within the same bridge network can communicate using container names or IP addresses.

### Key Features:
- Isolates containers from the host network.
- Allows communication between containers via IP or container names.
- **Limited to a single host** (does not support multi-host communication without additional setup).

![Bridge Network](https://user-images.githubusercontent.com/43399466/217745543-f40e5614-ac34-4b78-85a9-91b24512388d.png)

### Creating a Custom Bridge Network

To isolate containers from the default bridge network, you can create your own bridge network:

```sh
docker network create -d bridge my_bridge
```

Another example with a specific subnet:

```sh
docker network create --driver bridge --subnet 192.168.1.0/24 secure_bridge
```

### Attaching Containers to a Bridge Network

```sh
docker run -d --net=my_bridge --name db training/postgres
```

If two containers are attached to different bridge networks, they are **completely isolated** and cannot communicate. However, you can connect them later:

!{Isolated Connection](https://user-images.githubusercontent.com/43399466/217748680-8beefd0a-8181-4752-a098-a905ebed5d2a.png)

```sh
docker network connect my_bridge web
```

![Bridge Connection](https://user-images.githubusercontent.com/43399466/217748726-7bb347d0-3736-4f89-bdff-31d240b15150.png)

### Example Commands:

#### 1st Container (Login)
```sh
docker run -d --name login-container nginx:latest
docker exec -it login-container /bin/bash
apt update
apt-get install iputils-ping -y
ping <ip-address-of-logout-container>   # Should work
ping <ip-address-of-finance-container>  # Can't access it
```

#### 2nd Container (Logout)
```sh
docker run -d --name logout-container nginx:latest
docker exec -it logout-container /bin/bash
apt update
apt-get install iputils-ping -y
ping <ip-address-of-login-container>   # Should work
ping <ip-address-of-finance-container> # Can't access it
```

#### 3rd Container (Finance - Using a Secure Network)
```sh
docker network create secure-network
docker network ls
docker run -d --name finance-container --network=secure-network nginx:latest
```

To inspect container networks:
```sh
docker ps
docker inspect login-container
docker inspect logout-container
docker inspect finance-container
```

---
## Host Networking

The **host network** allows containers to use the host system's network stack **directly**, instead of creating a separate namespace.

### Key Features:
- Containers share the **same network namespace** as the host.
- The container **does not get its own IP**; it uses the host's IP.
- **Faster networking performance**, but **less isolation**.
- Used for performance-sensitive applications requiring high network performance.

### Running a Container with Host Networking

```sh
docker run --network="host" <image_name>:latest
```

Example:
```sh
docker run --name host-demo --network="host" nginx:latest
docker ps
docker inspect host-demo
```

**⚠️ Security Consideration:**
Using host networking reduces container isolation, so use it with caution.

---
## Overlay Networking

Overlay networks enable **multi-host** communication by allowing containers on different hosts to communicate securely.

### Key Features:
- Used in **Docker Swarm** for cross-host communication.
- Containers can communicate **securely** using an encrypted tunnel.
- Ideal for **distributed applications** running on multiple hosts.

---
## Macvlan Networking

Macvlan allows containers to appear as **physical devices** on the network rather than virtualized Docker containers.

### Key Features:
- Assigns a **unique MAC address** to each container.
- The container appears as a **separate physical machine**.
- Ideal for applications that require direct network access (e.g., legacy systems that need a dedicated IP).

---
## Conclusion

Docker provides several networking modes, each suited for different use cases:
- **Bridge Network:** Default, isolated, and limited to a single host.
- **Host Network:** Directly uses the host's network stack.
- **Overlay Network:** Enables communication across multiple hosts.
- **Macvlan Network:** Assigns a unique MAC address, making the container appear as a physical device.



