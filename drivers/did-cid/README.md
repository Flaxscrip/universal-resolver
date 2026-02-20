# Universal Resolver Driver: did:cid

A [Universal Resolver](https://github.com/decentralized-identity/universal-resolver) driver for the **did:cid** method used by [Archon Protocol](https://archetech.com).

## About did:cid

`did:cid` is a content-addressed DID method where the DID identifier is derived from the IPFS CID of the initial DID document. This provides:

- **Instant creation** — No blockchain transaction required
- **Self-certifying** — The DID proves its own integrity
- **Multi-registry** — Updates can be anchored to various registries (Hyperswarm, Bitcoin, etc.)

## Specifications

- DID Method: `did:cid`
- [DID Specification](https://github.com/nickthetj/archon-protocol/blob/main/docs/did-cid-spec.md)
- [Archon Protocol](https://archetech.com)

## Example DIDs

```
did:cid:bagaaieraxdxq4fm2kjh6yqjxjor3t2idczkmxd4v7in4u353fa6m6sms2pnq
did:cid:bagaaierajzwcicueqdkbgk75lgekdmvtbo5zv3spq2p4f7d7ow42urlwi32a
```

## Running Locally

```bash
# Install dependencies
npm install

# Start the driver
PORT=4250 ARCHON_GATEKEEPER_URL=http://localhost:4224 npm start

# Test resolution
curl http://localhost:4250/1.0/identifiers/did:cid:bagaaieraxdxq4fm2kjh6yqjxjor3t2idczkmxd4v7in4u353fa6m6sms2pnq
```

## Docker

```bash
# Build
docker build -t archetech/uni-resolver-driver-did-cid:0.1.0 .

# Run
docker run -p 4250:4250 -e ARCHON_GATEKEEPER_URL=http://host.docker.internal:4224 archetech/uni-resolver-driver-did-cid:0.1.0

# Test
curl http://localhost:4250/1.0/identifiers/did:cid:bagaaieraxdxq4fm2kjh6yqjxjor3t2idczkmxd4v7in4u353fa6m6sms2pnq
```

## Environment Variables

| Variable | Default | Description |
|----------|---------|-------------|
| `PORT` | `4250` | Port to listen on |
| `GATEKEEPER_URL` | `https://gatekeeper.archon.technology` | Archon gatekeeper endpoint |

## API Endpoints

| Endpoint | Description |
|----------|-------------|
| `GET /1.0/identifiers/{did}` | Resolve a did:cid |
| `GET /1.0/methods` | List supported methods (`["cid"]`) |
| `GET /health` | Health check |

## Integration with Universal Resolver

Add to `docker-compose.yml`:

```yaml
driver-did-cid:
  image: archetech/uni-resolver-driver-did-cid:0.1.0
  ports:
    - "4250:4250"
  environment:
    - ARCHON_GATEKEEPER_URL=http://your-gatekeeper:4224
```

Add to `application.yml`:

```yaml
- pattern: "^(did:cid:.+)$"
  url: "${uniresolver_web_driver_url_did_cid:http://driver-did-cid:4250/1.0/identifiers/}"
  testIdentifiers:
    - "did:cid:bagaaieraxdxq4fm2kjh6yqjxjor3t2idczkmxd4v7in4u353fa6m6sms2pnq"
    - "did:cid:bagaaierajzwcicueqdkbgk75lgekdmvtbo5zv3spq2p4f7d7ow42urlwi32a"
```

## Contact

- **Maintainer**: Archetech
- **Email**: contact@archetech.com
- **GitHub**: https://github.com/nickthetj/archon-protocol

## License

Apache 2.0
