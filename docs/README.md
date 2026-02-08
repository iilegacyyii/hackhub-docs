# Networking

Library containing utilities regarding Subnets, Ports, and IPs.

**Methods**
 - [GetSubnet](#getsubnet)
 - [IsIp](#isip)

**Classes**
 - [Port](#port)
 - [Subnet](#subnet)

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

**Properties**
 - [external](#external)
 - [internal](#internal)
 - [service](#service)
 - [target](#target)
 - [version](#version)

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

**Methods**
 - [GetPortData](#getportdata)
 - [GetPorts](#getports)
 - [GetRouter](#getrouter)
 - [PingPort](#pingport)

**Properties**
 - [ip](#ip)
 - [lanIp](#lanip)

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
const workstationSubnet = await Networking.GetSubnet(workstationIp);
const routerSubnet = await workStationSubnet.GetRouter();
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

# Shell

Library that allows you to manage the terminal.

**Methods**
 - [clear](#clear)
 - [GetArgs](#getargs)
 - [isLocked](#islocked)
 - [lock](#lock)
 - [unlock](#unlock)

**Classes**
 - [Directory](#directory)
 - [Process](#process) 

## clear

Clears the terminal.

**Definition**
```js
function Shell.clear(): void
```

**Example usage**
```js
Shell.clear()
```

## GetArgs

Get a list of command line arguments.

> Note: The parsing is very primitive, it's just a split on `<space>`.

**Definition**
```js
function Shell.GetArgs(): string[]
```

**Usage example**
```js
// -> string[], in this example -> ["192.168.1.1", "21"]
const args = Shell.GetArgs()

if (args.length < 2) {
	throw "Usage: scan <ip> <port>"
}

const ip = args[0]
const port = Number(args[1])
```

## isLocked

Returns `true` if the terminal input field is currently locked.

**Definition**
```js
function Shell.isLocked(): boolean
```

**Usage example**
```js
if (Shell.isLocked()) {
	Shell.unlock()
}
```

## lock

Unlock the terminial input field. Useful in combination with [unlock](#unlock) when waiting on commands that may take a while (e.g. lynx)

**Definition**
```js
function Shell.lock(): void
```

**Example usage**
```js
Shell.lock()
await Shell.Process.exec("lynx froj")
Shell.unlock()
```

## unlock

Unlock the terminial input field. Useful in combination with [lock](#lock) when waiting on commands that may take a while (e.g. lynx)

```js
function Shell.unlock(): void
```

**Example usage**
```js
Shell.lock()
await Shell.Process.exec("lynx froj")
Shell.unlock()
```

## Directory

Sub-library of [Shell](#shell) that allows you to `Get` and `Set` the shell's current working directory (CWD).

**Methods**
 - [Get](#get)
 - [Set](#set)

### Get

Returns a [File](#file) object containing information about the CWD.

_Note: This is a little buggy upon first terminal launch - the terminal assumes you're in your home directory when you first launch it (when you are actually in `/root`). This can be fixed by just `cd`-ing into any directory, and the state synchronises._

**Definition:**
```js
function Shell.Directory.Get(): FileSystem.File
```

**Example usage:**
```js
var cwd = Shell.Directory.Get()
println(`cwd: ${cwd.name}`)
```

### Set

Set the CWD to a given `DirectoryID`. Returns `true` on success.

_Note: This is currently not usable without abusing bugs since Directory ID's are not exposed via the intended in-game API. Developers are looking into it..._

**Definition:**
```js
function Shell.Directory.Set(DirectoryID: string): boolean
```

**Example usage:**
TODO: No idea yet, need to figure out how to find a directory ID.

## Process

Sub-library of [Shell](#shell) that allows you to print output and execute terminal commands.

**Methods**
 - [exec](#exec)
 - [println](#println)

### exec

Execute a terminal command.

**Definition**
```js
function Shell.Process.exec(stdin: string): Promise<void>
```

**Example usage**
```js
const target = "admin.victim.com";
Shell.Process.exec(`subfinder -d ${target}`)
```

### println

Print a line of terminal output. This appears to be the exact same as the globally available `println`.

**Definition**
```js
function Shell.Process.println(stdout: Stdout | Stdout[]): void
```

**Example usage**
```js
Shell.Process.println("haha i like frogs 🐸") // haha i like frogs 🐸
Shell.Process.println(["1", "2", 3, 0.123]) // 1230.123
```

# Metasploit

TODO: implement me