# NOTE 
Our torstorm service and it's torstorm.org website has been discontinued.   

This was mainly because we kept getting complaints from torstorm.org's web hoster and it's domain registrar about child pornography that was accessible from torstorm.org, because of .onion sites that were hosting such content.   
Whenever we received such complaints, I would change nginx.conf so that the particular .onion couldn't be accessed using torstorm, and I would explain that we have no control over the .onion site that originally hosts the content, and that the content could still be reached by using a similar service, or by visiting the .onion directly.   
Responding to the complaints became too time consuming, and since this was a free/non-essential service provided to everyone, we decided to discontinue it.   

cryptostorm members can still access .onion sites directly, without having to install any other software.

The files and the rest of this README are still here for historical purposes, and if anyone else wanted to recreate a service similar to torstorm.org

P.S.   
Nobody has ever commented on how we appear to be publishing our nginx private SSL key here, in the file client.key :/   
Of course, it wasn't our real private key. It was just some silly base64 encoded text that appears to be an RSA key.

# torstorm

torstorm is a free service provided by cryptostorm that provides access to .onion sites without requiring Tor browser, similar to what [Tor2web](https://tor2web.org/) offers.

`Note to cryptostorm members: Thanks to our `[DeepDNS](http://deepdns.net/)` setup, cryptostorm members can go directly to any .onion (or .i2p) site without torstorm.`

### Usage

To use the torstorm service, simply replace the `.onion` part of a `.onion` URL with `.torstorm.org`. Once you're on that .onion page, any further .onion links will be automatically replaced with .torstorm.org.

For example:  
https://3g2upl4pq6kufc4m.torstorm.org/  
(DuckDuckGo's 3g2upl4pq6kufc4m.onion page)

### Files

For the curious, these are the configs/scripts that make torstorm possible:

[nginx.conf](https://github.com/cryptostorm/torstorm/blob/master/nginx.conf) - The nginx config file that handles the initial requests. Has a basic setup block for the main torstorm website, plus another block that handles .onion requests and forwards to onion2web.lua. Also includes the useless (but neat) feature of randomly changing the webserver banner version for every request, just to mess with web vuln scanners.

[onion2web.lua](https://github.com/cryptostorm/torstorm/blob/master/onion2web.lua) - The lua script that nginx calls for .onion requests. The purpose of onion2web.lua is to replace all .onion links with .torstorm.org, and also to forward those requests to the tor SOCKS server running on 127.0.0.1:9050. It's the same code that's at https://github.com/starius/onion2web/blob/master/onion2web.lua except that our onion2web.lua also includes a bit that tries to remove any STUN/WebRTC javascript code that might be trying to leak your real IP. 

[socks5.lua](https://github.com/cryptostorm/torstorm/blob/master/socks5.lua) - required by onion2web.lua for SOCKS.

[tor](https://www.torproject.org/) - We use a basic tor setup for torstorm. SOCKS server listens on localhost.

[cert.key](https://github.com/cryptostorm/torstorm/blob/master/cert.key) - something to do with nginx, we're not sure.

`NOTE: An SSL wildcard certificate was required to get HTTPS to work for .onion links accessed through torstorm.org`
