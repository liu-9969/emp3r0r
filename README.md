# emp3r0r
linux post-exploitation

**This project is NOT finished**


<!-- vim-markdown-toc GFM -->

* [updates](#updates)
* [demo](#demo)
    * [reverse shell](#reverse-shell)
    * [port forwarding](#port-forwarding)
* [how to test](#how-to-test)
* [roadmap](#roadmap)
    * [features](#features)
        * [connection](#connection)
        * [agent](#agent)
        * [internal networks](#internal-networks)
    * [modules](#modules)
* [about tmux](#about-tmux)
    * [in case you don't know](#in-case-you-dont-know)
    * [key bindings](#key-bindings)
* [thanks](#thanks)

<!-- vim-markdown-toc -->

## updates

<a href="https://jm33.me/emp3r0r-0x00.html" target="_blank">https://jm33.me/emp3r0r-0x00.html</a>

## demo

### reverse shell

<p>
    <img width="600" src="/img/rshell.svg">
</p>

### port forwarding

<p>
    <img width="600" src="/img/portfwd.svg">
</p>

## how to test

```bash
git clone git@github.com:jm33-m0/emp3r0r.git

cd emp3r0r

cp .tmux.conf ~ # if you wish to use my tmux config

cd core
./build.py # select a target to build: ./build.py <cc/agent>
./emp3r0r # launch CC server (with a user interface)

# on the target linux machine
./agent
```

## roadmap

### features

#### connection

- [x] client-server structure, reverse connection
- [x] **HTTP2**, **full duplex** connection between agent and cc
- [x] **TLS**, with all security check enabled (trust additional CA generated by user)
- [x] dynamically generated CA and TLS certificates, making build process easier
- [ ] **use Cloudflare as C2** (or any other CDNs alike) if you like, since it supports HTTP2

#### agent

- [x] **LPE suggest** and auto root
- [x] an indicator for CC status, which can be used by agents to check if CC is online,
which, can be accessed via services like Github and Twitter, drawing less attention
- [x] persistence via various ways
- [ ] hide itself via libc-hijacking and syscall hijacking

#### internal networks

nowadays many linux hosts live in corporate networks **without direct connection to the internet**, how do we control them?
you might set up some port mapping or even run C2 inside their internal networks, but i have a different idea:

i can go further by making the whole thing a botnet:

- [ ] any hosts with internet connection set **port mapping to C2**
- [ ] any hosts without internet try to connect to the port mapping set by other agents, then get forwarded to C2
- [ ] if needed, the agent will take advantage of available proxies set by corporate admins
- [ ] emp3r0r can **exploit** some RCE vulnerabilities and weak/empty passwords in the internal network, so that it controls more hosts
- [ ] botnet feature can be disabled if not needed

### modules

- [x] `cmd` : execute shell command on target
- [x] `shell` : a basic command shell, with several helpers (a **real bash shell**, file uploading/downloading, vim, etc)
- [x] `lpe_suggest` : invoke [upc](https://github.com/pentestmonkey/unix-privesc-check/blob/master/upc.sh) and
[les](https://github.com/mzet-/linux-exploit-suggester), open their reports with `less` in new tmux window
- [x] `get_root` : automatic **privilege escalation**
- [ ] `lkm` : an **lkm** providing APIs for file/proc hiding, hidden backdoor, etc. automatically compiled for target kernel
- [ ] `libc_hijack` : upload a shared library (libemp.so) on target machine and make it `LD_PRELOAD`, so we can hijack many libc calls, providing similiar features like `lkm`, but more portable
- [ ] `injector` : **inject** code into running processes via `PTRACE`
- [x] `persistence` : get **persistence** via various methods
- [ ] `harvester` : **credentials** harvesting
- [ ] `data_exfil` : data exfiltration
- [x] `proxy` : socks5 **proxy over HTTP2**
- [x] `port_forward`: port mapping over HTTP2
- [ ] `containerized` : run code in a **container** (for better hiding)
- [ ] `evilkvm` : take advantage of kvm
- [ ] `vuln_scan`: discover and take over more targets in the network

## about tmux

### in case you don't know

emp3r0r utilizes [tmux](https://github.com/tmux/tmux/wiki) to provide features like remote editing, cmd output viewing.

if you wish to use my tmux config, you can put `.tmux.conf` under your `$HOME`

```
cp .tmux.conf ~
```

### key bindings


| Key Binding                | Description        |
|----------------------------|--------------------|
| <kbd>C-x - </kbd>          | Split vertically   |
| <kbd>C-x _ </kbd>          | Split horizontally |
| <kbd>C-x x </kbd>          | Kill current pane  |
| <kbd>C-x c </kbd>          | New tab            |
| <kbd>C-x [1,2,3...] </kbd> | Switch to tab      |
| <kbd>C-x , </kbd>          | Rename tab         |

legend:

- <kbd>C-x -</kbd> means <kbd>Ctrl</kbd> plus <kbd>X</kbd>, then <kbd>-</kbd>
- <kbd>[1,2,3...]</kbd> means any numeric key

## thanks

- [pty](https://github.com/creack/pty)
- [readline](https://github.com/bettercap/readline)
- [h2conn](https://github.com/posener/h2conn)
- [diamorphine](https://github.com/m0nad/Diamorphine)
- [Upgrading Simple Shells to Fully Interactive TTYs](https://blog.ropnop.com/upgrading-simple-shells-to-fully-interactive-ttys/)
