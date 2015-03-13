# torstorm

torstorm is a free service provided by cryptostorm that provides access to .onion sites without requiring Tor browser, similar to what [Tor2web](https://tor2web.org/) offers.

### Usage

To use the torstorm service, simply replace the `.onion` part of a `.onion` URL with `.torstorm.org`. Once you're on that .onion page, and further .onion links will be automatically replaced with .torstorm.org.

For example:  
https://3g2upl4pq6kufc4m.torstorm.org/  
(DuckDuckGo's 3g2upl4pq6kufc4m.onion page)

### Files

For the curious, these are the configs/scripts that make torstorm possible:

[nginx.conf](https://github.com/cryptostorm/torstorm/blob/master/nginx.conf) - The nginx config file that handles the initial requests. Has a basic setup block for the main torstorm website, plus another block that handles .onion requests and forwards to onion2web.lua. Also includes the useless (but neat) feature of randomly changing the webserver banner version for every request, just to mess with web vuln scanners.

[onion2web.lua](https://github.com/cryptostorm/torstorm/blob/master/onion2web.lua) - The lua script that nginx calls for .onion requests. Same code that's at https://github.com/starius/onion2web/blob/master/onion2web.lua except that ours also includes a bit that tries to remove any STUN/WebRTC javascript code that might be trying to leak your real IP. The purpose of onion2web.lua is to replace all .onion links with .torstorm.org, and also to forward those requests to the tor SOCKS server running on 127.0.0.1:9050.

[socks5.lua](https://github.com/cryptostorm/torstorm/blob/master/socks5.lua) - required by onion2web.lua for SOCKS.

[tor](https://www.torproject.org/) - We use a basic tor setup for torstorm. SOCKS server listens on localhost.

NOTE: An SSL wildcard certificate was required to get HTTPS to work for .onion links accessed through torstorm.org
