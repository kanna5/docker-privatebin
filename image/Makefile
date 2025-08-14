IMGNAME := kanna5/privatebin

image:
	docker build --pull -t $(IMGNAME) .

push:
	docker push $(IMGNAME)

.PHONY: image push
