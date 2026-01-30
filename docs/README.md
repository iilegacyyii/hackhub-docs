# Networking

Contains utilities regarding Subnets, Ports, and IPs.

> **Methods**
> - [GetSubnet](#getsubnet)
> - [IsIp](#isip)

**Classes**
> - [Port](#port)
> - [Subnet](#subnet)

## GetSubnet
Given an IP, returns a [Subnet](#subnet) if available.

**Definition:**
```js
function Networking.GetSubnet(ip: string): Promise<Networking.Subnet | null>
```

**Example usage:**
```js
const subnet = await Networking.GetSubnet(ip);
if (!subnet) throw "Error: Unable to connect to subnet!";

const portNumbers = await subnet.GetPorts();
if (!portNumbers.length) throw "Error: No ports found!";
```

## IsIP
Check if a given IP is valid or not. Returns `true` or `false` respectively.

**Definition:**
```js
function Networking.IsIp(ip: string): boolean
```

**Example usage:**
```js
const ip = await prompt("Enter IP address: ");

if (!Networking.IsIp(ip)) {
	throw "Error: Invalid IP address!";
}
```

## Port
Represents an in-game port.

> **Properties:**
> - [external](#external)
> - [internal](#internal)
> - [service](#service)
> - [target](#target)
> - [version](#version)

### external
The externally open port number (e.g. via the router).

**Definition:**
```js
(property) Networking.Port.external: number
```

**Example usage:**
```js
const port = await subnet.GetPortData(portNumber);
if (!port) {
	println(`Error: Unable to access port ${portNumber}`);
	continue;
}
const isOpen = (await subnet.PingPort(portNumber)) ? "OPEN" : "CLOSED";

var line = `${isOpen}\t${port.external}\t${port.target}\t${port.internal}\t${port.service}\t${port.version}`
println(line)
```

### internal
The port that [external](#external) routes to on the internal target host. Put simply, if `27.170.5.160:443` points to `192.168.1.2:4444` then `internal == 4444`.

**Definition:**
```js
(property) Networking.Port.external (number)
```

**Example usage:**
```js
const port = await subnet.GetPortData(portNumber);
if (!port) {
	println(`Error: Unable to access port ${portNumber}`);
	continue;
}
const isOpen = (await subnet.PingPort(portNumber)) ? "OPEN" : "CLOSED";

var line = `${isOpen}\t${port.external}\t${port.target}\t${port.internal}\t${port.service}\t${port.version}`
println(line)
```

### service
The service listening on the given port number, if there is one available.

**Definition:**
```js
(property) Networking.Port.service?: string | undefined
```

**Example usage:**
```js
const port = await subnet.GetPortData(portNumber);
if (!port) {
	println(`Error: Unable to access port ${portNumber}`);
	continue;
}
const isOpen = (await subnet.PingPort(portNumber)) ? "OPEN" : "CLOSED";

var line = `${isOpen}\t${port.external}\t${port.target}\t${port.internal}\t${port.service}\t${port.version}`
println(line)
```

### target

The internal IP address that the port number routes to.

**Definition:**
```js
(property) Networking.Port.target?: string | undefined
```

**Example usage:**
```js
const port = await subnet.GetPortData(portNumber);
if (!port) {
	println(`Error: Unable to access port ${portNumber}`);
	continue;
}
const isOpen = (await subnet.PingPort(portNumber)) ? "OPEN" : "CLOSED";

var line = `${isOpen}\t${port.external}\t${port.target}\t${port.internal}\t${port.service}\t${port.version}`
println(line)
```

### version

The version of the service listening on the port number.

**Definition:**
```js
(property) Networking.Port.version?: string | undefined
```

```js
const port = await subnet.GetPortData(portNumber);
if (!port) {
	println(`Error: Unable to access port ${portNumber}`);
	continue;
}
const isOpen = (await subnet.PingPort(portNumber)) ? "OPEN" : "CLOSED";

var line = `${isOpen}\t${port.external}\t${port.target}\t${port.internal}\t${port.service}\t${port.version}`
println(line)
```

**Example usage:**
```js
const port = await subnet.GetPortData(portNumber);
if (!port) {
	println(`Error: Unable to access port ${portNumber}`);
	continue;
}
const isOpen = (await subnet.PingPort(portNumber)) ? "OPEN" : "CLOSED";

var line = `${isOpen}\t${port.external}\t${port.target}\t${port.internal}\t${port.service}\t${port.version}`
println(line)
```

## Subnet
Represents an in-game subnet.

> **Methods:**
> - [GetPortData](#getportdata)
> - [GetPorts](#getports)
> - [GetRouter](#getrouter)
> - [PingPort](#pingport)

> **Properties:**
> - [ip](#ip)
> - [lanIp](#lanip)

### GetPortData
Given a port number on a subnet, returns a [Port](#port).

**Definition:**
```js
(method) Networking.Subnet.GetPortData(port: number): Promise<Networking.Port | null>
```

**Example usage:**
```js
for (const portNumber of portNumbers) {
	println(`Scanning port: ${portNumber}...`);
	
	const port = await subnet.GetPortData(portNumber);
	if (!port) {
		println(`Error: Unable to access port ${portNumber}`);
		continue;
	}
}
```

### GetPorts
Get a list of all port numbers on a subnet.

**Definition:**
```js
(method) Networking.Subnet.GetPorts(): Promise<number[]>
```

**Example usage:**
```js
const subnet = await Networking.GetSubnet(ip);
if (!subnet) throw "Error: Unable to connect to subnet!";

println("Fetching ports...");
const portNumbers = await subnet.GetPorts();

if (!portNumbers.length) throw "Error: No ports found!";

println(`Available ports: ${portNumbers.join(", ")}`);
```

### GetRouter
Returns the [subnet](#subnet) of the [subnet](#subnet)'s router.

> NOTE: Untested, seems like a strange API? Could maybe test with large story network?

**Definition:**
```js
(method) Networking.Subnet.GetRouter(): Promise<Networking.Subnet | undefined>
```

**Example usage:**
```js
// TODO: figure out a reasonable use case...
// Perhaps something like this?
const workstationSubnet = Networking.GetSubnet(workstationIp);
const routerSubnet = workStationSubnet.GetRouter();
println(`Router @ ${routerSubnet.ip} (${routerSubnet.lanIp})`)
```

### PingPort
Check if a port is open or closed. Returns `true` if open.

**Definition:**
```js
(method) Networking.Subnet.PingPort(port: number): Promise<boolean>
```

**Example usage:**
```js
for (const portNumber of portNumbers) {
	println(`Scanning port: ${portNumber}...`);
	
	const port = await subnet.GetPortData(portNumber);
	if (!port) {
		println(`Error: Unable to access port ${portNumber}`);
		continue;
	}

	const isOpen = await subnet.PingPort(portNumber);

	println("===============");
	println(`Port: ${portNumber}`);
	println(`Status: ${isOpen ? "OPEN" : "CLOSED"}`);
	if (port.service) println(`Service: ${port.service}`);
	if (port.version) println(`Version: ${port.version}`);
}
```

### ip
The [subnet](#subnet)'s public IP address.

**Definition:**
```js
(property) Networking.Subnet.ip: string
```

**Example usage:**
```js
println("Fetching subnet information...");
const subnet = await Networking.GetSubnet(ip);
if (!subnet) throw "Error: Failed to retrieve subnet information!";

println(`Subnet: ${subnet.ip}/${subnet.lanIp}`);
```

### lanIp
The [subnet](#subnet)'s LAN IP address.

**Definition:**
```js
(property) Networking.Subnet.lanIp: string
```

**Example usage:**
```js
println("Fetching subnet information...");
const subnet = await Networking.GetSubnet(ip);
if (!subnet) throw "Error: Failed to retrieve subnet information!";

println(`Subnet: ${subnet.ip}/${subnet.lanIp}`);
```

# Metasploit

TODO