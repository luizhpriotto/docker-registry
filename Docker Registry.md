# docker-registry

I use the "pwd" command because a bug when the bind of path is called for "yml" file.
Create a path /opt/registry
<pre>mkdir /opt/registry</pre>
<pre>docker run -d --name="registry" --restart="always" -v $(pwd)/config.yml:/etc/docker/registry/config.yml -v /mnt/registry:/var/lib/registry -e "VIRTUAL_HOST=registry.castrolanda.coop.br" -p 5000:5000 \
registry:2</pre>
