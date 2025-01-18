IMAGE=room386
VERSION=1.0

docker-build: Dockerfile cntnr-init
	env BUILDKIT_PROGRESS=plain \
	  docker build $(REBUILDFLAGS) -f Dockerfile \
	    --rm -t $(IMAGE):$(VERSION) -t $(IMAGE):latest .

# To force a complete clean build, do:
#   make rebuild
rebuild:
	rm -f $(BUNDLES)
	make REBUILDFLAGS="--no-cache --pull"

