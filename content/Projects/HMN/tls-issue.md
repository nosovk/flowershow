If you check working TLS connection, you may see some list of Extensions in the end:
![[normal-tls-connetcion.png]]
Actually we are interested in "server_name" extension. This one also called [SNI](https://wikipedia.org/wiki/Server_Name_Indication).

![[normal-tls-sni.png]]
Inside it, we can find hostname, that we want to connect.

And here we can check our connection from Delphi:

![[broken-tls-no-sni.png]]
As you can see, there is no SNI extension in request. Unfortunately, downgrading to SSL also would bot help, because we have to somehow identify which hostname we are willing to connect.
There is no option to connect to CloudFlare without SNI support, because they use one IP for multiple hostnames. 
Regarding Traefik, it has option to setup [default certificate](https://doc.traefik.io/traefik/https/tls/#default-certificate) that will be used for all connections as default, and changing to TLS1 there probably could help. But on my opinion it possesses security flaw.

I see 2 solutions.
## Enable SNI support at Delphi
According to that [article,](https://stackoverflow.com/questions/39545080/tidhttp-and-tls-sni-doesnt-work) SNI support have been introduced in r5321, on 11 Jan 2016. If the used version of Indy inside Delphi is newer, it should be reconfigured to connect to TLS1.0 with SNI extension.
## Provide local proxy
As Eugen mentioned, we can set up a local gate, that will receive HTTP connection, and will pack it into TLS tunnels. There are lots of tools capable to do that, for example [stunnel](https://www.stunnel.org/) or [socat](https://www.redhat.com/sysadmin/getting-started-socat)
