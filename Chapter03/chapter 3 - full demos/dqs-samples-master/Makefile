# samples for the Docker Quick Start guide
SHELL = bash

all: build-all
build-all:  from label copy add1 add2 add3 env arg1 arg2 arg3 user workdir run volume expose cmd entrypoint entrypoint2 healthcheck onbuild

from: 
	cd from-demo && \
	docker image build --rm --tag from-demo:1.0 .

run-from:
	docker container run --rm from-demo:1.0

label:
	cd label-demo && \
	docker image build --rm --tag label-demo:1.0 .

run-label:
	docker image inspect --format '{{json .Config}}' hello-earl:1.0.1 | jq '.Labels'

copy:
	cd copy-demo && \
	docker image build --rm --tag copy-demo:1.0 .

run-copy:
	docker container run --rm copy-demo:1.0 ls -l -R theqsg

add1:
	cd add1-demo && \
	docker image build --rm --tag add-demo:1.0 .

add2:
	cd add2-demo && \
	docker image build --rm --tag add-demo:2.0 .

add3:
	cd add3-demo && \
	docker image build --rm --tag add-demo:3.0 .

run-add:
	docker container run --rm add-demo:1.0 ls -l -R theqsg
	docker container run --rm add-demo:2.0 ls -l -R theqsg
	docker container run --rm add-demo:3.0

env:
	cd env-demo && \
	docker image build --rm --tag env-demo:1.0 .

run-env:
	docker image inspect --format '{{json .Config}}' env-demo:1.0 | jq '.Env'
	docker container run --rm --env changeMe="New Value" --env adhoc="run time"  env-demo:1.0 env

arg1:
	cd arg-demo && \
	docker image build --rm --tag arg-demo:1.0 . 

arg2:
	cd arg-demo && \
	docker image build --rm \
	   --build-arg key1="buildTimeValue" \
	   --build-arg key2="good till env instruction" \
	   --tag arg-demo:2.0 .

arg3:
	cd arg3-demo && \
	docker image build --rm \
	   --build-arg username=35 \
	   --build-arg appdir="/opt/hello" \
	   --tag arg-demo:3.0 .

run-arg1:
	docker image inspect --format '{{json .Config}}' arg-demo:1.0 | jq '.Env'
	docker container run --rm arg-demo:1.0 env

run-arg2:
	docker image inspect --format '{{json .Config}}' arg-demo:1.0 | jq '.Env'
	docker container run --rm arg-demo:2.0 env

run-arg3:
	docker container run --rm --env lifecycle="test" arg-demo:3.0

clean-arg1:
	docker image rm arg-demo:1.0

clean-arg2:
	docker image rm arg-demo:2.0

user:
	cd user-demo && \
	docker image build --rm --tag user-demo:1.0 .

run-user:
	docker container run --rm user-demo:1.0 id

workdir:
	cd workdir-demo && \
	docker image build --rm --tag workdir-demo:1.0 .

run:
	cd run-demo && \
	docker image build --rm --tag run-demo:1.0 .

run-run:
	docker container run --rm -it \
	   --name run-demo \
	   run-demo:1.0

volume:
	cd volume-demo && \
	docker image build --rm --tag volume-demo:1.0 .

run-volume1:
	docker container run --rm -it \
	   --mount source=myvolsrc,target=/myvol \
	   volume-demo:1.0

run-volume2:
	docker volume ls
	docker container run --rm -d --name vol-demo \
	   volume-demo:1.0 tail -f /dev/null
	docker volume ls
	docker container stop vol-demo
	docker container ls

run-volume3:
	@echo "***This sample will NOT work on OSX"
	$(eval rc=$(shell docker volume create myvolsrc))
	docker container run -d --name vol-demo \
		--mount source=myvolsrc,target=/myvol \
		volume-demo:1.0 tail -f /dev/null
	docker container exec vol-demo ls -l /myvol
	$(eval retstr=$(shell docker volume inspect myvolsrc -f "{{.Mountpoint}}"))
	#@echo "retstr: ${retstr}"
	$(eval filename:=$(shell echo ${retstr}/new-file.txt))
	#@echo "filename: ${filename}"
	@echo "***the touch command will fail on OSX"
	sudo touch ${filename}
	docker container exec vol-demo ls -l /myvol
	docker container stop vol-demo
	docker container rm vol-demo
	docker volume rm myvolsrc

expose:
	cd expose-demo && \
	docker image build --rm --tag expose-demo:1.0 .

run-expose1:
	@echo "****Dockerfile has EXPOSE, but not including -p in run"
	docker container run --rm -d --name expose-demo-noports \
	   expose-demo:1.0
	@echo "****The port command will show no ports open"
	docker port expose-demo-noports
	docker container stop expose-demo-noports

run-expose2:
	@echo "****Dockerfile has EXPOSE, and we are including -p in run"
	docker container run --rm -d --name expose-demo-withport80 \
	   -p 8888:80 \
	   expose-demo:1.0
	@echo "****The port command will show port 80 open throuh host port 8888"
	docker port expose-demo-withport80
	@echo "****Testing the port with curl should show a response"
	curl localhost:8888
	docker container stop expose-demo-withport80

run-expose3:
	@echo "****Dockerfile has EXPOSE, and we are including -P in run"
	docker container run --rm -d --name expose-demo-withallports \
	   -P \
	   expose-demo:1.0
	@echo "****The port command should show ALL EXPOSE ports"
	docker port expose-demo-withallports
	docker container stop expose-demo-withallports

cmd:
	cd cmd-demo && \
	docker image build --rm --tag cmd-demo:1.0 .

run-cmd:
	docker container run --rm -it --name cmd-demo cmd-demo:1.0

entrypoint:
	cd entrypoint-demo && \
	docker image build --rm --tag entrypoint-demo:1.0 .

entrypoint2:
	cd entrypoint2-demo && \
	docker image build --rm --tag entrypoint-demo:2.0 .

run-entrypoint:
	docker container run --rm --name my-mkdir entrypoint-demo:1.0

run-entrypoint2:
	docker container run --rm --name my-ping entrypoint-demo:2.0
	docker container run --rm --name my-ping entrypoint-demo:2.0 google.com

healthcheck:
	cd healthcheck-demo && \
	docker image build --rm --tag healthcheck-demo:1.0 .

run-healthcheck:
	docker container run --rm -d -p 80:80 --name health healthcheck-demo:1.0

onbuild:
	cd onbuild-my-base-demo && \
	docker image build --rm --tag my-base:1.0 .
	cd onbuild-my-app-demo && \
	docker image build --rm --tag my-app:1.0 .

run-onbuild:
	docker image inspect my-app:1.0
