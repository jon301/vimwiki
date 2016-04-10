# Meetup NightClazz Golang @ Zenika (2016-02-04)

## Go

https://github.com/Zenika/NightClazz-Go-lang-

### Tooling
- godoc (godoc.org) : extracts and generates documentation for Go programs
- vet : examines Go source code and reports suspicious constructs
- oracle : source analysis tool that answers questions about Go programs
- golint : prints out sytyle mistakes, is concerned with coding style
- gofmt : reformats Go source code
- generate : scanning for special comments in Go source code that identify general commands to run
- gorename : performs precise type-safe renaming of identifiers
- -race & racy : race detector
- godef, gocode, impel, ...

### Install a dependency
- go get github.com/jon301/my-package

### Update a dependency
- go get -u github.com/jon301/my-package

### Concurrency
- goroutine : Lightweight thread managed by the Go runtime


## IPFS

With HTTP, you search for _locations_.
With IPFS, you search for _content_.

Filesystem distribué basé sur Git, en P2P.


## Docker

### Volumes
- Performance
- Partage de données entre host et conteneur
- Partage de données entre conteneurs
- Usage : BDD, Configurations, logs, sauvegardes ...

### Volumes plugins (1.9)
- Etendre les fonctionnalités de Docker (e.g. les volumes) en donnant la possibilité de le faire par des plugins
- Comment ? .sock (unix socket), .spec (url), .json (plugin spec.) ; API HTTP ; TCP ou unix socket

### Volume plugins API
- Handshake API
- Volume API

https://github.com/docker/go-plugins-helper


## Workshop Go

package flag

var toto = 'titi' // var est important si tu n'es pas dans une fonction
toto := 'titi' // := fonctionne uniquement à l'intérieur d'une fonction

