# plugin-builder

Open Meridian's registry builder. The platform dispatches this workflow to build a plugin version from its publisher's signed `vX.Y.Z` tag, scan it (Trivy, ClamAV), attach SLSA provenance, a bill of materials and metadata as OCI referrers, sign with the platform's KMS key, and report the results back to the platform.

- The publisher's build runs with read-only access and no OIDC token. Only the `sign` job, in the `signing` environment on `main`, can request a token and reach the key.
- Every action is pinned by SHA and every image by digest.

See the registries pages at https://open-meridian.dev for how publishing works.
