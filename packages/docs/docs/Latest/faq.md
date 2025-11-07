# FAQ

Q: pnpm install reports error `Cannot find matching keyid: ${JSON.stringify({ signatures, keys })}`

A: Run `COREPACK_INTEGRITY_KEYS=0 corepack prepare` first, then execute `pnpm install`.
