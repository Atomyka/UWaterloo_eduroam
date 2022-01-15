# UWaterloo_eduroam
_eduroam template for fellow arch users at UWaterloo!_


The joys of connecting to eduroam... a rite of initiation upon arrival at university, a rich set of difficulties shared by student communities across the globe. With the risk of destroying this bonding experience, I hope the following will make the process of connecting to eduroam not just easier but perhaps even a little interesting :)

## How-To
The most basic, fool-proof method of connecting on Linux I have found is via [iwd](https://wiki.archlinux.org/title/Iwd). Install the iwd package and follow the commands under "Usage - Connect to a Network". However, [you will need](https://wiki.archlinux.org/title/Iwd#eduroam) a special .8021x file for this to work, as is the case for any wifi set-up using WPA Enterprise (see below for some details behind this perhaps cryptic term). This file is precisely the template in this Git repo. Just replace the placeholders with your own [username] and [password] and save in /var/lib/iwd.

The second file is the digital certificate, whose purpose is also lightly explained below. Actually, you will already have a folder with all certificates on your machine, located in /etc/ca-certificates/extracted/cadir. [UWaterloo](https://uwaterloo.atlassian.net/wiki/spaces/ISTKB/pages/262012990/Connecting+to+Eduroam+-+General+Overview) specificies which specific certificate to use (GlobalSignRootCA - R3). In case you are unsure, the certificate (ca.pem) is uploaded here for you to cross-check.

## Under the Hood
But what actually do those specifications mean? At the core of secure wifi is [WPA](https://en.wikipedia.org/wiki/Wi-Fi_Protected_Access) - Wifi Protected Access. It is one of the core security programs overseen by the Wifi-Alliance, a non-profit bundle of companies based in Texas. Depending on who/where you are - privately using wifi at home or browsing in the network of a larger organization, you will be using either WPA-Personal or WPA-Enterprise. 

<img title="EAP setup" alt="EAP setup" src="https://upload.wikimedia.org/wikipedia/commons/1/1f/802.1X_wired_protocols.png">
*Source: [Wikipedia](https://en.wikipedia.org/wiki/IEEE_802.1X)* 

The latter is relevant for eduroam, so let's take a closer look: Structurally, there are 3 components in the connection process: You (or rather your laptop), an authenticator and an authentication server. Picture that server as the guardian of the network - you have to prove your worthiness to the server to gain access. How to succeed? Using your username, password and the digital certificate. Combined together, these will allow you to perform a handshake with the server. And what is the right etiquette to talk to this server? It's the [MSCHAPV2](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-chap/4740bf05-db7e-4542-998f-5a4478768438) protocol, developed by Microsoft. 

If you take a look in the .8021x file, that's the EAP-Method. What's [EAP](https://en.wikipedia.org/wiki/Extensible_Authentication_Protocol)? Extensible Authentication Protocol. A lot of protocols, you're probably thinking. Might seem overkill, but it's important to be aware of this landscape of mechanisms which the people at IEEE and elsewhere work to create in order for us to communicate securely across the world.

One final note: The internet is build up in [layers](https://en.wikipedia.org/wiki/Internet_layer). To ensure safety, not all information is shared between all layers. You can think of each layer as a blackblox that takes care of a specific task and can rely on the other layers to do their job through so-called [encapsulation](https://en.wikipedia.org/wiki/Encapsulation_(networking)). In the case of EAP, this protocol is encapsulated in the IEEE 8021x standard. I'm just mentioning this to explain why on earth the template is a .8021x file. It's simply one buidling block in the big puzzle of secure Internet!
