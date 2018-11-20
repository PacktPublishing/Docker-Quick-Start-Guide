# samples for the Docker Quick Start guide
SHELL = bash

all: build-all
build-all:  volume

volume:
	cd volume-demo2 && \
	docker image build --rm --tag volume-demo2:1.0 .

run-volume4: volume
	docker volume ls
	docker container run --rm -d \
	   --mount source=my-vol-02,target=/myvol \
	   --name vol-demo2 \
	   volume-demo2:1.0 tail -f /dev/null

run-volume5: volume
	docker volume ls
	docker container run --rm -d --name vol-demo2 \
	   volume-demo2:1.0 tail -f /dev/null

run-volume6: volume
	docker volume ls
	echo "important data" > /tmp/data-file.txt
	docker container run --rm -d \
	   -v /tmp/data-file.txt:/myvol/data-file.txt \
	   --name vol-demo2 \
	   volume-demo2:1.0 tail -f /dev/null

run-volume7: volume
	# Step 1
	docker container run \
	   --rm -d \
	   -v data-vol-01:/data/vol1 -v data-vol-02:/data/vol2 \
	   --name data-container \
	   volume-demo2:1.0 tail -f /dev/null
	# Step 2
	docker container run \
	   --rm -d \
	   --volumes-from data-container \
	   --name app-container \
	   volume-demo2:1.0 tail -f /dev/null
	# Prove it
	docker container exec app-container ls -l /data
	# Prove it more
	docker container inspect -f '{{ range .Mounts }}{{ .Name }} {{ end }}' app-container
	# Clean up
	docker container stop app-container data-container
	docker volume prune --force

clean:
	docker container stop vol-demo2
	docker container ls
