IMG_NAME := week_1_tax_data/analysis
DOCKER_BUILD_CONTEXT="."

.PHONY: build-analysis
build-analysis:
	docker build \
	-f $(DOCKER_BUILD_CONTEXT)/analysis.Dockerfile \
	-t $(IMG_NAME) .

.PHONY: analysis
analysis: build-analysis
	docker run -i --network host \
		-v "$(shell pwd):/work" -w /work \
		-e PYTHONPATH=/work/src \
		$(IMG_NAME) \
		jupyter lab \
			--ip='0.0.0.0' --port=8888 \
			--no-browser --allow-root \
			--NotebookApp.token='' --NotebookApp.password=''
