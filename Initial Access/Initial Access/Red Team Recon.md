# Red Team Recon

```jsx
whois [thmredteam.com](http://thmredteam.com/)
```

DNS queries can be executed with many different tools found on our systems, especially Unix-like systems. One common tool found on Unix-like systems, Windows, and macOS is nslookup. In the following query, we can see how nslookup uses the default DNS server to get the A and AAAA records related to our domain.

```jsx

nslookup [cafe.thmredteam.com](http://cafe.thmredteam.com/)
```

Another tool commonly found on Unix-like systems is dig, short for Domain Information Groper (dig). dig provides a lot of query options and even allows you to specify a different DNS server to use. For example, we can use Cloudflare's DNS server: dig @1.1.1.1 [tryhackme.com](http://tryhackme.com/). 

```jsx

dig [cafe.thmredteam.com](http://cafe.thmredteam.com/) @1.1.1.1
```

host is another useful alternative for querying DNS servers for DNS records. Consider the following example.

```jsx

host [cafe.thmredteam.com](http://cafe.thmredteam.com/)
```

The final tool that ships with Unix-like systems is traceroute, or on MS Windows systems, tracert. As the name indicates, it traces the route taken by the packets from our system to the target host.

```jsx

traceroute [cafe.thmredteam.com](http://cafe.thmredteam.com/)
```

### Advance searching

| Symbol / Syntax | Function |
| --- | --- |
| `"search phrase"` | Find results with exact search phrase |
| `OSINT filetype:pdf` | Find files of type `PDF` related to a certain term. |
| `salary site:blog.tryhackme.com` | Limit search results to a specific site. |
| `pentest -site:example.com` | Exclude a specific site from results |
| `walkthrough intitle:TryHackMe` | Find pages with a specific term in the page title. |
| `challenge inurl:tryhackme` | Find pages with a specific term in the page URL. |

### Some search engines, such as Google, provide a web interface for advanced searches:

***[Google Advanced Search (opens in new tab)](https://www.google.com/advanced_search).

Other times, it is best to learn the syntax by heart, such as

*** [Google Refine Web Searches (opens in new tab)](https://support.google.com/websearch/answer/2466433)

*** [DuckDuckGo Search Syntax (opens in new tab)](https://help.duckduckgo.com/duckduckgo-help-pages/results/syntax/)

*** [Bing Advanced Search Options (opens in new tab)](https://help.bing.microsoft.com/apex/index/18/en-US/10002).

Combining advanced Google searches with specific terms, documents 
containing sensitive information or vulnerable web servers can be found.
 Websites such as [Google Hacking Database (opens in new tab)](https://www.exploit-db.com/google-hacking-database)
 (GHDB) collect such search terms and are publicly available. Let's take
 a look at some of the GHDB queries to see if our client has any 
confidential information exposed via search engines. GHDB contains 
queries under the following categories:

- **Footholds**Consider [GHDB-ID: 6364 (opens in new tab)](https://www.exploit-db.com/ghdb/6364) as it uses the query `intitle:"index of" "nginx.log"` to discover Nginx logs and might reveal server misconfigurations that can be exploited.
- **Files Containing Usernames**For example, [GHDB-ID: 7047 (opens in new tab)](https://www.exploit-db.com/ghdb/7047) uses the search term `intitle:"index of" "contacts.txt"` to discover files that leak juicy information.
- **Sensitive Directories**For example, consider [GHDB-ID: 6768 (opens in new tab)](https://www.exploit-db.com/ghdb/6768), which uses the search term `inurl:/certs/server.key` to find out if a private RSA key is exposed.
- **Web Server Detection**Consider [GHDB-ID: 6876 (opens in new tab)](https://www.exploit-db.com/ghdb/6876), which detects GlassFish Server information using the query `intitle:"GlassFish Server - Server Running"`.
- **Vulnerable Files**For example, we can try to locate PHP files using the query `intitle:"index of" "*.php"`, as provided by [GHDB-ID: 7786 (opens in new tab)](https://www.exploit-db.com/ghdb/7786).
- **Vulnerable Servers**For instance, to discover SolarWinds Orion web consoles, [GHDB-ID: 6728 (opens in new tab)](https://www.exploit-db.com/ghdb/6728) uses the query `intext:"user name" intext:"orion core" -solarwinds.com`.
- **Error Messages**Plenty of useful information can be extracted from error messages. One example is [GHDB-ID: 5963 (opens in new tab)](https://www.exploit-db.com/ghdb/5963), which uses the query `intitle:"index of" errors.log` to find log files related to errors.

Example:

```jsx
filetype:xls site:clinic.thmredteam.com
```

```jsx
passwords site:clinic.thmredteam.com
```

### **Specialized Search Engines**

```jsx
[https://viewdns.info/](https://viewdns.info/)
```

```jsx
[https://threatintelligenceplatform.com/](https://threatintelligenceplatform.com/)
```

**Censys**

```jsx
[https://search.censys.io/](https://search.censys.io/)
```

**Shodan**

To use Shodan from the command-line properly, you need to create an account with [Shodan (opens in new tab)](https://www.shodan.io/), then configure `shodan` to use your API key using the command, `shodan init API_KEY`.

You can use different filters depending on the [type of your Shodan account (opens in new tab)](https://account.shodan.io/billing). To learn more about what you can do with `shodan`, we suggest that you check out [Shodan CLI (opens in new tab)](https://cli.shodan.io/).

## Installation

The command-line interface (**CLI**) for Shodan is provided alongside the [Python library](https://github.com/achillean/shodan-python).
 This means that you need to have Python installed on your computer in 
order to use the Shodan CLI. Once you have Python configured then you 
can run the following command to install the Shodan CLI:

```bash
$pip install -U --user shodan
```

To confirm that it was properly installed you can run the command:

```bash
$shodan
```

It should show you a list of possible sub-commands for the Shodan CLI.

Finally, initialize the Shodan CLI with [your API key](https://account.shodan.io/):

```bash
$shodan init YOUR_API_KEY
```

Done! You are now ready to use the CLI and try out the examples.

### Troubleshooting

#### shodan: command not found

If the installation succeeded without errors but you're unable to run
 the command then try closing and re-opening the terminal window.

#### pip: command not found

This means that you either don't have Python properly installed or that you're running an older version which doesn't include `pip`. Try using one of these alternate commands to install the Shodan CLI:

```bash
$easy_install -U --user shodan
```

Or if your system only has the `pip3` tool:

```bash
$pip3 install -U --user shodan
```

#### Let’s demonstrate a simple example of looking up information about one of the IP addresses we got from `nslookup cafe.thmredteam.com`. Using `shodan host IP_ADDRESS`, we can get the geographical location of the IP address and the open ports, as shown below.

```bash
           pentester@TryHackMe$ shodan host 172.67.212.249

172.67.212.249
City:                    San Francisco
Country:                 United States
Organisation:            Cloudflare, Inc.
Updated:                 2021-11-22T05:55:54.787113
Number of open ports:    5

Ports:
     80/tcp
    443/tcp
	|-- SSL Versions: -SSLv2, -SSLv3, -TLSv1, -TLSv1.1, TLSv1.2, TLSv1.3
   2086/tcp
   2087/tcp
   8080/tcp
```

### [Recon-ng](https://github.com/lanmaster53/recon-ng)   framework

```jsx
recon-ng -w thmredteam
```

#### Workflow

```jsx
recon-ng -w thmredteam
db insert domains
marketplace search domains-
marketplace info google_site_web
marketplace install google_site_web
modules load google_site_web
options list
run
```

## [Maltego](https://www.maltego.com/)  is an application that blends mind-mapping with OSINT