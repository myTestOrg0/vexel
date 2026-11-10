# Organization VEX

Store applicability assessments in OpenVEX documents. A document can contain multiple statements; each statement names its vulnerability, status and allowed products.

## Limit an exception to repositories

For our application integration, a product ID is the exact repository URL. Put the exact dependency PURL under that product's `subcomponents`:

```json
"products": [
  {
    "@id": "https://github.com/myTestOrg0/dao-voting-ui",
    "subcomponents": [{"@id": "pkg:npm/lodash@4.17.11"}]
  },
  {
    "@id": "https://github.com/myTestOrg0/another-approved-repo",
    "subcomponents": [{"@id": "pkg:npm/lodash@4.17.11"}]
  }
]
```

This fragment illustrates scope only; it is not an approved exception for either repository. The deployed lodash statements remain `under_investigation` and do not suppress findings.

The Socket wrapper obtains the current repository from GitHub Actions `GITHUB_REPOSITORY`. It requires an exact product URL and exact component/version/CVE match. An exception naming only the package, a different repository, or a wildcard does not suppress a finding in our application integration. Copying the workflow/configuration to another repository does not copy exception scope.

Multiple allowed repositories are separate entries in `products`. There is no custom `repositories` extension to interpret. `status_notes` can link to the analysis; a link alone does not set scope.

## Effective VEX

The wrapper selects the first applicable statement in the order own → Docker DHI → Aqua, after product eligibility checks. `affected` and `under_investigation` stop lower-priority suppression; `not_affected` and `fixed` suppress the matched finding. Unmatched findings retain their scanner policy action.

For the current application repository it emits `effective-vex.json` with only selected statements and that repository's scope. Source links and original statement timestamps are retained. If no statements match, no empty OpenVEX document is emitted.

Current Socket application scans use only own VEX. DHI is eligible only for confirmed DHI images; Aqua is restricted to Docker images by organization policy. The real image integration remains deferred pending registry access. Its effective output must additionally bind to the exact image digest/platform.

OpenVEX syntax alone does not guarantee that another scanner enforces GitHub repository scope. Consumers must implement the same scope check, or receive only an already filtered, artifact-specific effective document. Do not give a global repository feed to a consumer that ignores product context.
