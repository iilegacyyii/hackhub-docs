# Networking

Contains utilities regarding Subnets, Ports, and IPs.

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

**Properties:**
- [external](#external)
- [internal](#internal)
- [service](#service)
- [target](#target)
- [version](#version)

### external
The externally open port (e.g. via the router)

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
The service hosted on the port, if there is one available.

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

The internal IP address that the open port routes to (if forwarded).

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

The version of the service running on the port.

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

# Metasploit

TODO