## This is an Fork project
Click on link to see <a target="_blank" href="https://github.com/tanrax/maza-ad-blocking">Maza Original Post</a> 
🧑‍🎨 Credits <a target="_blank" href="https://andros.dev/">Andros Fenollosa</a> 

<img alt="banner" src="media/banner.png" width="100%">

Like Pi-hole but local and using your operating system.

Simple, native and efficient **local ad blocker**. Bash script compatible with **MacOS**, **Linux**, **BSD** and **Windows Subsystem for Linux (WSL)**.

- Just **bash** 🤖.
- It affects **any browser** or software installed 😱.
- You **don't have to install any browser extensions or applications** 🚫, you just use the tools of your operating system.
- You update the list of DNS to be blocked with a **single command** 😎.
- Pure **Opensource** ❤️.

## Before and After Maza
- Preview
<img alt="demo" src="media/demov2.jpg">

## Help me continue to improve

<p align="center">
  <a href='https://ko-fi.com/heryxin' target='_blank'><img height='36' style='border:0px;height:36px;' src='https://cdn.ko-fi.com/cdn/kofi2.png?v=2' border='0' alt='Buy Me a Coffee at ko-fi.com' /></a>
</p>

## 📋 Requirements

- **bash** 4.0 or higher
- **curl**
- macOS users, **gsed**: `brew install gnu-sed`

## ⚙️ How to Install

``` bash
curl -o maza https://raw.githubusercontent.com/HeribertoKubuntu/maza-ad-blocking/refs/heads/master/maza && chmod +x maza && sudo mv maza /usr/local/bin
```

Optional but recommended, make a backup of your hosts file.

``` bash
sudo cp /etc/hosts /etc/hosts.backup
```

## 📟 Commands List
|Command            |Description|
|:-----------------:|:-----------------------------------:|
|status             |Check if it's active or not          |
|update             |Update the list of DNS to be blocked |
|start              |Activate blocking DNS                |
|stop               |Stop blocking DNS                    |
|autostart          |Create a "plist" to run on startup   |
|disableautostart   |Delete "plist"                       |
|--help             |Display this usage message and exit  |

### Example of execution 

- Update database

``` bash
maza update
```

- Start

``` bash
maza start
```

## ✅ Ignore domains

#### 1. Create a file named `ignore` inside the Maza configuration folder.

``` bash
touch ~/.maza/ignore
```

#### 2.- Edit the file for all domains that you do not want to ignore with a line break.

Example:

``` txt
ads-twitter.com
ads.twitter.com
```

#### 3.- Finally update Maza to apply the changes.

``` bash
maza update
```

## 🚫 blocking domains

#### 1. Create a file named `blocking` inside the Maza configuration folder.

``` bash
touch ~/.maza/blocking
```

#### 2.- Edit the file for all domains that you do not want to ignore with a line break.

Example:

``` txt
ads-twitter.com
ads.twitter.com
```

#### 3.- Finally update Maza to apply the changes.

``` bash
maza update
```

## 🔒 Alternative DNS list

By default the Yoyo DNS list (Peter Lowe) is used. If you want to use another list, like Steven Black's for example, you must modify the variable in line 7.

It would go from:

```
URL_DNS_LIST="https://pgl.yoyo.org/adservers/serverlist.php?showintro=0&mimetype=plaintext"
```

To the following:

```
URL_DNS_LIST="https://raw.githubusercontent.com/StevenBlack/hosts/master/hosts"
```

## DNSMASQ

Unfortunately the hosts file does **not support sub-domains (wildcards)**, which is necessary to correctly filter all DNS. You will **need to install locally a server** for that purpose, Maza supports the **Dnsmasq** format.

[MacOS](#MacOS)

### MacOS

- Update Maza

```bash
maza update
```

- Install

```bash
brew install dnsmasq
```

- Configure

Edit the file.

```
/usr/local/etc/dnsmasq.conf
```

Add the following line at the end.

```
conf-file=(your user path)/.maza/dnsmasq.conf
```

- Example

```
conf-file=/Users/myuser/.maza/dnsmasq.conf
```

- Start DNSMASQ

```bash
sudo brew services stop dnsmasq
sudo brew services start dnsmasq
```

- Tell your OS to use your DNS server

Delete the list of macOS DNS servers and add the 3 addresses. The first one will be your local server, and the other 2 belong to OpenDNS, which you can use any other.

```bash
127.0.0.1
208.67.222.222
208.67.220.220
```

<img alt="network macos" src="media/network-osx.jpg" width="500">

- Refresh your DNS cache

```bash
sudo dscacheutil -flushcache; sudo killall -HUP mDNSResponder
```

- Restart/Start Maza

```bash
sudo maza stop
sudo maza start
```

## Bonus: dnsmasq is in charge of solving all DNS

Add in configure file: `/usr/local/etc/dnsmasq.conf`

```
no-resolv
server=208.67.222.222
server=208.67.220.220
```

## 🔪 Uninstall
Easy uninstallation just copy/paste this command on terminnal

``` bash
sudo rm /usr/local/bin/maza && sudo rm -r ~/.maza
```

## Optionals
### Bonus: dnsmasq have `localhost` domains

If you want all your `.localhost` domains, for example, point to localhost add in configure file: `/usr/local/etc/dnsmasq.conf` or `/etc/dnsmasq.conf`.

```
address=/.localhost/127.0.0.1
```

### 🤖 Auto update of domains to be blocked

Open your `cron`.

``` bash
crontab -e
```

Add the following line at the end.

```
@daily maza update
```

## ⚠️ CAUTION

Remember to make a backup copy of `/etc/hosts` in case of unforeseen circumstances, neither the project nor its author will be responsible for any possible repercussions derived from not carrying out this action.
