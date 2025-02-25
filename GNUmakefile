.PHONY: \
	all \
	update \
	build \
	install \
	lint \
	vet \
	fmt \
	fmtcheck \
	clean \
	pretest \
	test

SRCS = $(shell git ls-files '*.go')
PKGS = $(shell go list ./... | grep -v /vendor/)
COVERAGE_PKGS = $(shell go list ./... | grep -Ev "/testing|/examples")

all: build test

build: main.go
	go build -o go-cpe $<


lint:
	@ go get -v github.com/golang/lint/golint
	$(foreach file,$(SRCS),golint $(file) || exit;)

vet:
	go vet $(PKGS) || exit;

fmt:
	gofmt -w $(SRCS)

fmtcheck:
	@ $(foreach file,$(SRCS),gofmt -s -l $(file);)
cov:
	sh coverage.sh $(COVERAGE_PKGS) || exit;
	go tool cover -html=coverage.out -o coverage.html

clean:
	go clean $(shell glide nv)

pretest: vet fmtcheck

test: pretest
	@ $(foreach pkg,$(PKGS), go test $(pkg) || exit;)
