build:
	docker build -t training/ansible:ubuntu -f Dockerfile.ubuntu .
	docker build -t training/ansible:centos -f Dockerfile.centos .

build-cserver:
	docker build -t training/ansible:cserver -f Dockerfile.cserver .

build-tserver:
	docker build -t training/ansible:tserver_centos -f Dockerfile.tserver.centos .
	docker build -t training/ansible:tserver_ubuntu -f Dockerfile.tserver.ubuntu .

build-all:
	make build
	make build-cserver
	make build-tserver

run-web-az-a:
	docker rm -f webaza || true
	docker run -h webaza --name webaza -itd training/ansible:tserver_ubuntu

run-web-az-b:
	docker rm -f webazb || true
	docker run -h webazb --name webazb -itd training/ansible:tserver_centos

run-app-az-a:
	docker rm -f appaza || true
	docker run -h appaza --name appaza -itd training/ansible:tserver_ubuntu

run-app-az-b:
	docker rm -f appazb || true
	docker run -h appazb --name appazb -itd training/ansible:tserver_centos

run-db-az-a:
	docker rm -f dbaza || true
	docker run -h dbaza --name dbaza -itd training/ansible:tserver_ubuntu

run-db-az-b:
	docker rm -f dbazb || true
	docker run -h dbazb --name dbazb -itd training/ansible:tserver_centos

run-control-server:
	docker rm -f cserver || true
	docker run -h cserver --name cserver --link webaza:webaza --link webazb:webazb --link appaza:appaza --link appazb:appazb --link dbaza:dbaza --link dbazb:dbazb -itd training/ansible:cserver /bin/bash

run-all:
	make run-web-az-a
	make run-web-az-b
	make run-app-az-a
	make run-app-az-b
	make run-db-az-a
	make run-db-az-b
	make run-control-server

start-lab:
	make build-all
	make run-all
