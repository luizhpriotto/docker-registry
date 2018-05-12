# Docker Registry

I use the "pwd" command because a bug when the bind of path is called for "yml" file.
Create a path "/opt/registry".
<pre>#mkdir /opt/registry</pre>
Paste "config.yml" here.
Before execute Docker run, create a directory for bind repository of registry.
<pre>#mkdir /mnt/registry</pre>
Now, run registry container.
<pre>#docker run -d --name="registry" --restart="always" -v $(pwd)/config.yml:/etc/docker/registry/config.yml -v /mnt/registry:/var/lib/registry -e "VIRTUAL_HOST=registry.domain.com" -p 5000:5000 \
registry:2</pre>
Registry can access only localhost, so, we going to create a proxy reverse with apache in Centos 7.
<pre>#yum -y install httpd mod_ssl</pre>
Execute:
<pre>#echo "LoadModule ssl_module modules/mod_ssl.so" > /etc/httpd/conf.modules.d/00-ssl.conf</pre>
Create a simple access file control:
<pre>#mkdir /opt/registry/auth</pre>
<pre>#docker run \
  --entrypoint htpasswd \
  registry:2 -Bbn testuser testpassword > /opt/registry/auth/htpasswd</pre>
Put your certificates in:
<pre>/opt/registry/certs</pre>
Paste a Virtual Host conf in:</pre>
<pre>/etc/conf.d/</pre>
Now, start httpd:
<pre>#systemctl start httpd</pre>
And configure to start on boot system:
<pre>#systemctl enable httpd</pre>
If necessary, configure the S.O. to accept the CA:
<pre>#cp /etc/registry/certs/gsorganizationvalsha2g2r1.cer /etc/pki/ca-trust/source/anchors/
#update-ca-trust
#systemctl restart docker</pre>

# Use Docker Registry

Log in the private registry:
<pre>#docker login registry.domain.com</pre>

Tag a Image:
<pre>docker tag c92568032ea9 registry.domain.com/wordpress_example:latest</pre>

Now, Push:
<pre>docker push registry.castrolanda.coop.br/wordpress_example:latest</pre>

Success?