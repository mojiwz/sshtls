<h1 align="center" tabindex="-1" dir="auto"><a id="user-content-xpanel" class="anchor" aria-hidden="true" tabindex="-1" href="#"></a>OPENSSH (SSH-TLS)</h1>
<h6 align="center" tabindex="-1" dir="auto"><a id="user-content-xpanel" class="anchor" aria-hidden="true" tabindex="-1" href="#"></a>add openssh in server</h6>
# Telegram Channel:
https://t.me/stunnel4

# Step 1: Update and Upgrade
<pre class="notranslate"><code>apt update && apt upgrade -y</code></pre>

# Step 2: Install Stunnel

Install Stunnel package using the code below:
<pre class="notranslate"><code>apt-get install stunnel4 -y</code></pre>

# Step 3: Configure Stunnel on the VPS

Stunnel configures itself using a file called "stunnel.conf" located by default in "/etc/stunnel". Create a “stunnel.conf” file in the “/etc/stunnel” folder using the following command:
<pre class="notranslate"><code>nano /etc/stunnel/stunnel.conf</code></pre>

Now we need to enter the ssl certificate path in the stunnel.conf file with the editor
<pre class="notranslate"><code>cert = /etc/stunnel/stunnel.pem</code></pre>

you must tell Stunnel to listen on which port for that service. This can be any of the ports, as long as it’s not blocked by another service or firewall:
<pre class="notranslate"><code>[openssh]
accept = sshtls_port
connect = 0.0.0.0:port</code></pre>

Finally, our file should look like this:
<pre class="notranslate"><code>cert = /etc/stunnel/stunnel.pem
[openssh]
accept = sshtls_port
connect = 0.0.0.0:port
</code></pre>


# Step 4: Create SSL Certificates

Stunnel uses SSL certificate to secure its connections, which you can easily create using the OpenSSL package:
Note: When creating the certificate, you will be asked for some information such as country and state, which you can enter whatever you like but when asked for “Common Name” you must enter the correct host name or IP address of your droplet (VPS).
<pre class="notranslate"><code>openssl genrsa -out key.pem 2048
openssl req -new -x509 -key key.pem -out cert.pem -days 1095
cat key.pem cert.pem >> /etc/stunnel/stunnel.pem</code></pre>

# Step 5: Enable Stunnel Service

Also, enable auto launch of Stunnel by running the following command in terminal
<pre class="notranslate"><code>sed -i 's/ENABLED=0/ENABLED=1/g' /etc/default/stunnel4</code></pre>

# Step 6: Restart Stunnel Service

Finally, restart Stunnel for configuration to take effect, using this command:
<pre class="notranslate"><code>service stunnel4 restart</code></pre>
