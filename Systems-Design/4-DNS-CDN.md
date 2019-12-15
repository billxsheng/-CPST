# DNS & CDN
## DNS (Domain Name Server)
- A DNS translates a domain name to an IP address
- NS (Name Server) record specifies the DNS servers for your domain
- MX (Mail Exchange) record specifies the mail servers for accepting messages
- A record (address) points a name to an IP address
- CNAME (canonical) points a name to another name or CNAME or to an A record
- CloudFlare and Route53 provide managed DNS services. 

### Disadvantage
- accessing a DNS introduces slight delay
DNS server management could be complex
- DDoS attack (denial of service)

## CDN (Content Delivery Network)
- globally distributed network of proxy servers, serving content from locations closer to the user
- generally, static files are served from CDN
- improves performance by allowing users to receive content from nearby data centers and servers do not have to serve requests 
### Push CDNs
- receive new content whenever changes occur on your server
- you are responsible for providing content, uploading to the CDN, and rewriting URLs
### Pull CDNs
- grabs new content when first user requests the content
- you leave the content on your server and rewrite URLs
- results in slower request until the content is cached
### Disadvantage
- Costs could be high depending on traffic
