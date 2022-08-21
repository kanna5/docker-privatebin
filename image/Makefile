IMGNAME := kanna5/privatebin

image:
	DOCKER_BUILDKIT=1 docker build --no-cache -t $(IMGNAME) .

push:
	docker push $(IMGNAME)

.PHONY: image push
