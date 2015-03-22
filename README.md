## Go by Example
[Go by Example](https://gobyexample.com)을 위한 내용,  툴체인, 웹서버



### 개요

예제파일들에서 코드와 주석을 뽑아 Go by Example site 는 만들어졌으며, 
사이트 `templates`를 통해서 그 데이터들을 렌더링 하였습니다. 
이러한 빌드 프로세스를 구현하는 프로그램은 `tools` 에 있습니다. 

The Go by Example site is built by extracting code &
comments from source files in `examples` and rendering
that data via the site `templates`. The programs
implementing this build process are in `tools`.

빌드프로세스는 현대HTTP 서버를 운영하는데 적합한 정적파일들의 디렉터리  `public` 을 생성합니다. 
우리는 `server.go` 에 경령화 Go Server를 포함합니다. 
The build process produces a directory of static files -
`public` - suitable for serving by any modern HTTP server.
We include a lightweight Go server in `server.go`.


### Building

To build the site:

```console
$ go get github.com/russross/blackfriday
$ tools/build
$ open public/index.html
```

To build continuously in a loop:

```console
$ tools/build-loop
```


### Local Deploy

```bash
$ mkdir -p $GOPATH/src/github.com/mmcgrana
$ cd $GOPATH/src/github.com/mmcgrana
$ git clone git@github.com:mmcgrana/gobyexample.git
$ cd gobyexample
$ go get
$ foreman start
$ foreman open
```


### Platform Deploy

Basic setup:

```bash
$ export DEPLOY=$USER
$ export APP=gobyexample-$USER
$ heroku create $APP -r $DEPLOY
$ heroku config:add -a $APP
    BUILDPACK_URL=https://github.com/mmcgrana/buildpack-go.git
    CANONICAL_HOST=$APP.herokuapp.com \
    FORCE_HTTPS=1 \
    AUTH=go:byexample
$ heroku labs:enable dot-profile-d -a $APP
$ git push $DEPLOY master
$ heroku open -a $APP
```

Add a domain + SSL:

```bash
$ heroku domains:add $DOMAIN
$ heroku addons:add ssl -r $DEPLOY
# order ssl cert for domain
$ cat > /tmp/server.key
$ cat > /tmp/server.crt.orig
$ curl https://knowledge.rapidssl.com/library/VERISIGN/ALL_OTHER/RapidSSL%20Intermediate/RapidSSL_CA_bundle.pem > /tmp/rapidssl_bundle.pem
$ cat /tmp/server.crt.orig /tmp/rapidssl_bundle.pem > /tmp/server.crt
$ heroku certs:add /tmp/server.crt /tmp/server.key -r $DEPLOY
# add ALIAS record from domain to ssl endpoint dns
$ heroku config:add CANONICAL_HOST=$DOMAIN -r $DEPLOY
$ heroku open -r $DEPLOY
```


### License

This work is copyright Mark McGranaghan and licensed under a
[Creative Commons Attribution 3.0 Unported License](http://creativecommons.org/licenses/by/3.0/).

The Go Gopher is copyright [Renée French](http://reneefrench.blogspot.com/) and licensed under a
[Creative Commons Attribution 3.0 Unported License](http://creativecommons.org/licenses/by/3.0/).


### Translations

Contributor translations of the Go by Example site are available in:

* [Chinese](http://everyx.github.io/gobyexample/) by [everyx](https://github.com/everyx)


### Thanks

Thanks to [Jeremy Ashkenas](https://github.com/jashkenas)
for [Docco](http://jashkenas.github.com/docco/), which
inspired this project.
