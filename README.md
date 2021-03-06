<h1 align="center">
  <br>
  Pion DTLS
  <br>
</h1>
<h4 align="center">A Go implementation of DTLS</h4>
<p align="center">
  <a href="https://sourcegraph.com/github.com/pions/dtls?badge"><img src="https://sourcegraph.com/github.com/pions/dtls/-/badge.svg" alt="Sourcegraph Widget"></a>
  <a href="http://gophers.slack.com/messages/pion"><img src="https://img.shields.io/badge/join-us%20on%20slack-gray.svg?longCache=true&logo=slack&colorB=brightgreen" alt="Slack Widget"></a>
  <br>
  <a href="https://travis-ci.org/pions/dtls"><img src="https://travis-ci.org/pions/dtls.svg?branch=master" alt="Build Status"></a>
  <a href="https://godoc.org/github.com/pions/dtls"><img src="https://godoc.org/github.com/pions/dtls?status.svg" alt="GoDoc"></a>
  <a href="https://coveralls.io/github/pions/dtls"><img src="https://coveralls.io/repos/github/pions/dtls/badge.svg" alt="Coverage Status"></a>
  <a href="https://goreportcard.com/report/github.com/pions/dtls"><img src="https://goreportcard.com/badge/github.com/pions/dtls" alt="Go Report Card"></a>
  <a href="https://www.codacy.com/app/Sean-Der/dtls"><img src="https://api.codacy.com/project/badge/Grade/18f4aec384894e6aac0b94effe51961d" alt="Codacy Badge"></a>
  <a href="LICENSE"><img src="https://img.shields.io/badge/License-MIT-yellow.svg" alt="License: MIT"></a>
</p>
<br>

Go DTLS 1.2 implementation. The original user is pion-WebRTC, but we would love to see it work for everyone.

A long term goal is a professional security review, and maye inclusion in stdlib.

# Goals/Progress
This will only be targeting DTLS 1.2, and the most modern/common cipher suites.
We would love contributes that fall under the 'Planned Features' and fixing any bugs!

# Current features
* DTLS 1.2 Client/Server
* Forward secrecy using ECDHE; with curve25519 and nistp256 (non-PFS will not be supported)
* AES_128_GCM
* Packet loss and re-ordering is handled during handshaking
* Key export (RFC5705)

# Planned Features
* Extended master secret support (RFC7627)
* Chacha20Poly1305
* AES_256_CBC

# Excluded Features
* DTLS 1.0
* Renegotiation
* Compression

# How to use
Pion DTLS can connect to itself and OpenSSL.

## Pion DTLS
For a DTLS 1.2 Server that listens on 127.0.0.1:4444
```sh
go run cmd/listen/main.go
```

For a DTLS 1.2 Client that connects to 127.0.0.1:4444
```sh
go run cmd/dial/main.go
```

## OpenSSL
```
  // Generate a certificate
  openssl ecparam -out key.pem -name prime256v1 -genkey
  openssl req -new -sha256 -key key.pem -out server.csr
  openssl x509 -req -sha256 -days 365 -in server.csr -signkey key.pem -out cert.pem

  // Use with cmd/dial/main.go
  openssl s_server -dtls1_2 -cert cert.pem -key key.pem -accept 4444

  // Use with cmd/listen/main.go
  openssl s_client -dtls1_2 -connect 127.0.0.1:4444 -debug -cert cert.pem -key key.pem
```

