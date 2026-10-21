# Next + Netlify Starter

Minimal [Next.js](https://nextjs.org/) 16 + React 19 starter wired for Netlify, with
Cypress end-to-end tests and GitHub Actions CI.

This repository began as a copy of
[`netlify-templates/next-netlify-starter`](https://github.com/netlify-templates/next-netlify-starter)
and has been modified since. The upstream MIT licence is restored in `LICENSE`.

## Requirements

- Node.js 20.9 or newer (`.nvmrc` pins 20; `engines` allows `>=20.9 <23`)
- npm

## Getting started

```bash
npm ci          # install exact, lockfile-pinned dependencies
npm run dev     # http://localhost:3000
```

## Scripts

| Script | What it does |
| --- | --- |
| `npm run dev` | Next.js dev server on port 3000 |
| `npm run build` | Production build (`.next/`) |
| `npm start` | Serve the production build on port 3000 |
| `npm test` | Run Cypress end-to-end tests headlessly against `http://localhost:3000` |
| `npm run test:open` | Open the Cypress test runner |

## Project layout

```
pages/            _app.js, _document.js, index.js, 404
components/       Header, Footer (+ CSS module)
styles/           globals.css, imported once from pages/_app.js
cypress/e2e/      end-to-end specs
netlify.toml      build command, Node version, security headers, Cypress plugin
.github/workflows CI: npm ci -> build -> Cypress
tina/             retained TinaCMS config (not used by the site - see below)
content/posts/    Markdown used by that retained Tina demo
```

Path aliases `@components/*` and `@styles/*` are configured in `jsconfig.json`. This is a
JavaScript project, so `jsconfig.json` - not `tsconfig.json` - owns the aliases; adding a
`tsconfig.json` silently overrides it. **CSS cannot be imported through an alias**, so
`pages/_app.js` imports `../styles/globals.css` by relative path.

## Environment variables

The site builds and runs with none. See `.env.example` for the optional TinaCMS values.

## TinaCMS

`tina/config.ts`, `tina/tina-lock.json` and `content/posts/hello-world.md` are kept as a
reference for the demo that shipped with this starter. The demo page was removed because
it imported `tina/__generated__/client`, which only exists after a successful
`tinacms build` and requires Tina Cloud credentials - so a fresh clone could not build.
Nothing in the running site depends on Tina today.

To switch it back on: `npm i -D tinacms @tinacms/cli`, add a page that uses the generated
client, put `tinacms build` back in front of `next build`, and set
`NEXT_PUBLIC_TINA_CLIENT_ID` and `TINA_TOKEN`.

## Deployment

Netlify builds with `npm run build` and serves `.next` (see `netlify.toml`). Security
headers (`X-Frame-Options`, `X-Content-Type-Options`, `Referrer-Policy`,
`Permissions-Policy`, HSTS) are set for all routes. A Content-Security-Policy is
deliberately not set yet: Next.js injects inline bootstrap scripts, so a CSP needs nonces
or hashes and should be tested before it is turned on.

## Testing

Cypress specs live in `cypress/e2e`. `cypress.config.js` points at
`http://localhost:3000`, matching `npm run dev` and `npm start`. CI builds the app,
serves it with `npm start`, waits for the port, then runs the specs.

## License

MIT - see `LICENSE`.
