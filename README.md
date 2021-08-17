![architecture diagram](https://github.com/Camouflage-Project/architecture/blob/master/singleproxy.png?raw=true)

# System Actors

### Customers
Customers are entities requiring IP address rotation when sending HTTP requests toward servers via the Internet. An example of a Customer would be a firm specializing in big data and web scraping.

### Cloud Nodes
A cloud node consists of a set of dockerized services that can be deployed by anyone on infrastructure of their choice. These services are responsible for managing Customer and Residential Proxy information, monitoring smart contract transactions, as well as proxying requests to Residential Proxies.

### Residential Proxies
A Residential Proxy consists of an executable installed as a service on an individual's home machine. This program opens an SSH tunnel to a particular Cloud Node's server, thereby allowing the Cloud Node to forward traffic to the Residential Proxy despite the Residential Proxy being located behind NAT.

# Flow

The Customer constructs an HTTP request, specifying a particular Cloud Node to be used as the HTTP proxy. In the cURL client, this is done using the `-x` or `--proxy` flag. The Customer also sends a special API key in an HTTP header and uses a certificate previously received from the Cloud Node.

Upon receiving this request, the Cloud Node decrypts TLS and reads the API key header, using this API key to identify the Customer. If the Customer has unused bandwidth in their account, the Cloud Node proxies the Customer's HTTP request to one of the Residential Proxies connected to that Cloud Node.

The Residential Proxy receives the HTTP request and proxies it to the destination server specified by the Customer, which handles the request, returning a response which is then proxied back via the Residential Proxy and Cloud Node to the Customer.
