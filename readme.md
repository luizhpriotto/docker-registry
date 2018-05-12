# Docker Registry

I use the "pwd" command because a bug when the bind of path is called for "yml" file.
Create a path /opt/registry
<pre>mkdir /opt/registry</pre>
Paste "config.yml" here.
Before execute Docker run, create a directory for bind repository of registry.
<pre>mkdir /mnt/registry</pre>
Now, run registry container.
<pre>docker run -d --name="registry" --restart="always" -v $(pwd)/config.yml:/etc/docker/registry/config.yml -v /mnt/registry:/var/lib/registry -e "VIRTUAL_HOST=registry.domain.com" -p 5000:5000 \
registry:2</pre>
