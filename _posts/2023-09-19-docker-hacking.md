---
title: Docker hacking
categories: [docker,shodan,security]
tags: [docker,containers,shodan,security,hacking]
---

************************************************************
************************************************************
Website NOT active anymore, migrated to https://cryptrz.org/ 
************************************************************
************************************************************

Since 2013, <a href="https://www.docker.com" target="_blank">Docker</a> has been a game changer in different IT industries in several ways, it gave both developers and users a lot of flexibility for developping and using many apps and operating systems.

<b>Firstly</b>, Docker containers provide isolation and portability for software applications. By encapsulating an application and its dependencies within a container, developers can ensure consistent behavior across different environments. This eliminates the notorious “works on my machine” problem and streamlines the deployment process.

<b>Secondly</b>, Docker enables the deployment of containers in clusters, managed by frameworks like Google’s Kubernetes. This approach allows for the separation of application code and infrastructure, facilitating highly resilient and elastic architectures. Container clustering is particularly beneficial for microservices-based applications, as it promotes scalability and fault tolerance.

<b>Lastly</b>, Docker containers offer a higher layer of abstraction for application deployment. They simplify the process of configuring, saving, and sharing server environments. With Docker, installing an application or large software can be as easy as running a few commands. This ease of use enhances productivity and accelerates development cycles.

While Docker has gained significant popularity in recent years, it does introduce some complexity to the development process, but also some weakness if you enable the remote access and use it with default settings. An attacker can then be <b>root</b> in a second, as we'll see below. 

# **Remote access for Docker daemon**

If you want to work remotely on a container, it's possible to <b>configure Docker</b> to accept requests from a remote host as explained on <a href="https://docs.docker.com/engine/security/protect-access/" target="_blank">this page from the Docker documention</a>. Even if the documentation explains how to protect Docker by creating <a href="https://docs.docker.com/engine/install/linux-postinstall/#manage-docker-as-a-non-root-user" target="_blank">a non-root user</a> or <a href="https://docs.docker.com/engine/security/protect-access/" target="_blank">protecting the daemon socket</a> for example, a lot of remotely accessible containers are used with the default configuration, accessible on <b>port 2375</b>, but also on <b>port 2376 for TLS</b> using a <b>root account</b>.

# **Find these containers on Shodan**

By searching for <code>product:docker port:2375</code> on <a href="https://shodan.io" target="_blank">Shodan</a>, we can see that many servers hosting containers with the <b>port 2375</b> open. 

![shodan-results](/assets/images/2023-09-19-docker-hacking/shodan-results.png) 

Because Shodan’s free accounts provide limited results (2 pages), you can increase the number of available results by filtering and specifying alternatively different countries by adding <code>country=XX</code> where <code>XX</code> represents the country code: <code>“country=US”</code> for USA, <code>“country=UK”</code> for United Kingdom, <code>“country=CN”</code> for China, etc… Complete list on <a href="https://www.iso.org/obp/ui/#search" target="_blank">iso.org</a>.

![shodan-results](/assets/images/2023-09-19-docker-hacking/shodan-search.png) 

# **Analyse the server before attacking (Optional)**

When you use Docker remotely, you can use the usual options listed on <a href="https://docs.docker.com/engine/reference/commandline/dockerd/" target="_blank">this page</a>. The difference is you need to spocify the host with the "<code>-H</code>" parameter. We will check first the <b>Docker</b> version installed by using the "<code>--version</code>" option on the first server listed on Shodan in the previous section. Then, we can list <b>all images</b> installed and available with the <code>images</code> parameter.

![docker-version](/assets/images/2023-09-19-docker-hacking/docker-version-images.png)


# **Launch the attack**

For listing every process actually running, you will use the "<code>ps</code>" parameter. You can see the operating system running on the container, its <b>uptime</b>, <b>size</b>, and espacially its <b>image ID</b> which we will use. 

![docker-ps](/assets/images/2023-09-19-docker-hacking/docker-ps.png)

Let's try the first one, running <a href="https://ubuntu.com/" target="_blank">Ubuntu</a>. After <code>docker -H IP_ADDRESS</code>,  we can select a container with the "<code>exec</code>" parameter, then add the "<code>-it</code>" options for an interactive shell ("<code>i</code>" for <b>interactive</b> and "<code>t</code>" for <a href="https://en.wikipedia.org/wiki/Tty_(Unix)" target="_blank">tty</a>), and the <b>image ID</b>. Finally, we can write what do we want to use on this container, here "<code>/bin/bash</code>".

![](/assets/images/2023-09-19-docker-hacking/docker-exec.png)

After a few seconds, <b>we are root</b>. No credentials, no confirmation, nothing. Just :

<i>"Hello that's me!<br>
-OK, please come and do whatever you want"</i>. 

Now, maybe you’ll need more tools. To install them, you’ll probably need <b>wget</b>, <b>curl</b> or <b>git</b>. On this container, <b>curl</b> is not available, you can install it with: <code>apt install curl -y</code>

![curl](/assets/images/2023-09-19-docker-hacking/curl-install.png)

Same for <b>wget</b>: <code>apt install wget -y</code>

![wget](/assets/images/2023-09-19-docker-hacking/wget-install.png)

The <b>git</b> command is already available, on this container. If not available you can install it with: <code>apt install git -y</code>

![git](/assets/images/2023-09-19-docker-hacking/git-help.png)

You can now imagine what a malicious attacker can do with all of these, like launching a <a href="https://en.wikipedia.org/wiki/Denial-of-service_attack" target="_blank">DDoS attack</a> executed from this container, <a href="https://en.wikipedia.org/wiki/Nmap" target="_blank">scanning</a> anonymously any sensitive server and coming back later to download results, creating a <a href="https://en.wikipedia.org/wiki/Phishing" target="_blank">phishing attack</a> or <a href="https://en.wikipedia.org/wiki/Clickjacking" target="_blank">clickjacking webpage</a>, etc…

If your Docker containers are remotely accessible, please check the security section in the official documentation and make it secure: <a href="https://docs.docker.com/engine/security/" target="_blank">https://docs.docker.com/engine/security/</a>
