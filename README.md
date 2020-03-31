# docker-proxy-with-registry
docker-proxy-with-registry


        docker run -d --name docker_registry_proxy -it \
       -p 0.0.0.0:3128:3128 \
       -v $(pwd)/docker_mirror_cache:/docker_mirror_cache \
       -v $(pwd)/docker_mirror_certs:/ca \
       -e REGISTRIES="k8s.gcr.io gcr.io docker.io" rpardini/docker-registry-proxy:0.2.4

 
 
Client:


     mkdir -p /etc/systemd/system/docker.service.d
      cat << EOD > /etc/systemd/system/docker.service.d/http-proxy.conf
    [Service]
    Environment="HTTP_PROXY=http://103.9.138.166:3128/"
    Environment="HTTPS_PROXY=http://103.9.138.166:3128/"
    EOD


curl http://103.9.138.166:3128/ca.crt > /usr/share/ca-certificates/docker_registry_proxy.crt

echo "docker_registry_proxy.crt" >> /etc/ca-certificates.conf

      update-ca-certificates --fresh
      systemctl daemon-reload
      systemctl restart docker.service



docker pull gcr.io/google_containers/etcd-amd64:3.2.24 
