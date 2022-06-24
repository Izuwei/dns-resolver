# DNS resolver

Dns program, which sends queries to DNS servers and lists received responses to standard output in a readable form. The assembly and analysis of DNS packets is implemented directly in the dns program. Only UDP communication is considered. In the implementation were used BSD sockets and the program is executable on the operating system GNU/Linux. On the standard output is printed information whether the obtained response is authoritative, whether it was obtained recursively and whether the response was shortened. It also lists all the sections and their records received in the response. For each section in the response is listed its name and the number of obtained records. For each record is given its name, type, class, TTL and data.

The program recognizes type A, AAAA, CNAME, NS, PTR, SOA, WKS, MX, TXT, SRV, and ANY records in responses and is able to fully deal with type A, AAAA, CNAME, PTR, NS, and SOA records. I was unable to catch other types of responses (than those fully supported) during testing. If any can be captured later, the data is treated with the string "Not supported type". Other unsupported existing types are treated with the string "UNKNOWN".

## Run
- For compilation use `make`.
- The order of parameters is arbitrary.
- Parameters in square brackets are optional.

```
./dns [-r] [-x] [-6] -s server [-p port] address
```

Where:
- **-r**: Required recursion (Recursion Desired = 1), otherwise without recursion.
- **-x**: Reverse query instead of direct.
- **-6**: Query type AAAA instead of default A.
- **-s** `server`: Domain name or IP address of the server to which the query is to be send.
- **-p** `port`: The port number to which the query is to be send (default 53).
- `address`: Queried address.

## Example

```  
./dns -r -s kazi.fit.vutbr.cz www.fit.vut.cz
```

> Authoritative: No, Recursive: Yes, Truncated: No  
> Question section (1)  
>   www.fit.vut.cz., A, IN  
> Answer section (1)  
>   www.fit.vut.cz., A, IN, 14400, 147.229.9.26  
> Authority section (0)  
> Additional section (0)  
> $ dns -r -s kazi.fit.vutbr.cz www.ietf.org  
> Authoritative: No, Recursive: Yes, Truncated: No  
> Question section (1)  
>   www.ietf.org., A, IN  
> Answer section (3)  
>   www.ietf.org., CNAME, IN, 300, www.ietf.org.cdn.cloudflare.net.  
>   www.ietf.org.cdn.cloudflare.net., A, IN, 300, 104.20.1.85  
>   www.ietf.org.cdn.cloudflare.net., A, IN, 300, 104.20.0.85  
> Authority section (0)  
> Additional section (0)  
