# Creating CI/CD Pipeline with GCP

If we look at the landscape of cloud, one thing it has undoubtedly revolutionized is how we design and deploy our applications. From the large monoliths just a decade ago, to well-containerized and scalable solutions, we have come a long way. Google has a nice [comic strip](https://cloud.google.com/kubernetes-engine/kubernetes-comic) describing this journey and the reasons for it. Still a lot of time continues to be spent on analyzing, building, and debugging this pipeline. To save some of that and for quick prototyping of applications deployed on Kubernetes on GCP, the following reference architecture can be used and adapted to the specific needs of a project.


## Overview
```
![Deployment Diagram](https://github.com/itsamk/gcp-application-pipeline/blob/main/assets/gcp-cd.png?raw=true)
```
The above diagram represents various cloud components that are used in this pipeline. Here are the links to those for more information –

Kubernetes - https://cloud.google.com/kubernetes-engine

Cloud Load Balancing - https://cloud.google.com/load-balancing

Managed Certificate - https://cloud.google.com/load-balancing/docs/ssl-certificates/google-managed-certs

DNS Server - https://www.cloudflare.com/en-ca/learning/dns/what-is-a-dns-server/

Cloud Build - https://cloud.google.com/build/docs

Artifacts Registry - [https://cloud.google.com/artifact-registry](https://cloud.google.com/artifact-registry)

Static IP - https://cloud.google.com/compute/docs/ip-addresses/reserve-static-external-ip-address