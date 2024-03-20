# Docker-X-Kubernetes-X-Openshift
Notes from my experience learning Docker Kubernetes and Openshift.

Changes To Upcoming Assignment
In the next video, you'll get a fun assignment on learning how Docker networks handle DNS and load balancing.

There have been a few changes to the images and commands in that assignment, so once you've watched the assignment, you can come back here for updated instructions.

Changes


elasticsearch newer versions require environment variable changes to work correctly.

centos is no longer a supported image by Red Hat, so we can use alpine instead for Linux utilities like nslookup and curl.

More Info
The :2 tag for elasticsearch is old, but it was simple to run on x86_64 machines. However, the older v2  doesn't have an arm64 build (M1, M2, Apple Silicon, Raspberry Pi, etc.) It's one of the last lectures for me to replace with an image that has an arm64 variant as well as the typical x86_64, so if you're on a arm64 machine, you can use one of these two options to finish the assignment:

There are two options.

Option 1: Use a more updated version of elasticsearch, but you'll have to set memory requirements and change some default environment variables to make it work as a simple single container. Remember you'll also want to change the network and alias for this assignment, but you can add the -e options similar to this: docker run -e "discovery.type=single-node" -e "ES_JAVA_OPTS=-Xms512m -Xmx512m" -e "xpack.security.enabled=false" --network <your_network> -d --network-alias search elasticsearch:8.4.3

Then you could start another container to run nslookup and curl

docker run --rm -it --network <your_network> alpine
# use nslookup to see both container IPs for the same DNS A record
nslookup search
# install curl and run it
apk add curl
curl search:9200
curl search:9200
You'll notice the new elasticsearch no longer has random cluster_name, so you'll need to look at cluster_uuid to tell them apart.

Option 2: you can replace the image name with bretfisher/httpenv (which runs on port 8888, not 9200) and get the same effect in this assignment. It's just a simple web server that returns the environment variables (including the container name to tell them apart) in HTTP.


