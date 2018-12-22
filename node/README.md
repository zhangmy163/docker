docker build -t testnode:8.11.3 -f Dockerfile .

docker run -d --name card -p 5757:5757 -v /root/docker_project/node/server:/release/card/server -v /root/docker_project/node/bak_node:/release/card/build/bak_node testnode:8.11.3 "dev" 