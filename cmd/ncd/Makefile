version := $(shell cat VERSION)
# Setup the -ldflags option for go build here, interpolate the variable values
LDFLAGS:=-ldflags "-w -s -X main.Version=$(version)"

build: main.go VERSION
	env GOOS=linux GOARCH=arm GOARM=7 go build ${LDFLAGS} -o ../../dist/ncd .
	@upx ../../dist/ncd

build.upload: guard-PROXY_HOST ncd
	ssh root@$(PROXY_HOST) mount -o remount,rw /
	scp ../../dist/ncd root@$(PROXY_HOST):/var/local/conf/cfg/autoload.d/ 
	ssh root@$(PROXY_HOST) reboot

guard-%:
	@ if [ -z "${${*}}" ]; then \
		echo "Environment variable $* not set"; \
		exit 1; \
	fi
