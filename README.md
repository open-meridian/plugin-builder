# plugin-builder

Open Meridian's registry builder. The platform dispatches this workflow to build a plugin version from its publisher's signed `vX.Y.Z` tag, scan it (Trivy, ClamAV), attach SLSA provenance, a bill of materials and metadata as OCI referrers, sign with the platform's KMS key, and report the results back to the platform.

- The publisher's build runs with read-only access and no OIDC token. Only the `sign` job, in the `signing` environment on `main`, can request a token and reach the key.
- Every action is pinned by SHA and every image by digest.

See the registries pages at https://open-meridian.dev for how publishing works.

## The signing smoke test

`smoke_test` proves the signing path without a publish: neither `build` nor `sign` runs, and a `smoke` job in the `signing` environment attests a throwaway image, held in a registry on its own runner, with the platform's KMS key, verifies the envelope the way the platform's intake does, and prints the key's public half. It never calls the platform. Started on `main` by someone with write access; the other inputs are required by the form and read by nothing in this mode:

```sh
gh workflow run build.yaml -R open-meridian/plugin-builder --ref main -f smoke_test=true \
  -f build_id=smoke -f repository=- -f tag=- -f listing=- -f registry=- -f image=- \
  -f intake_url=- -f intake_nonce=- -f base_images=- -f community_licence=-
```
