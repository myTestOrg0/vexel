# Socket POC decisions and priority tests

`pkg/npm/lodash/vex.json` records **under_investigation** for the Voting product.
It deliberately does not suppress any real finding. The existing Debian/DHI
statements are unchanged.

The fixtures in this directory are synthetic evaluator tests. They use
`urn:socket-poc:priority-test`, not the real Voting product. Names `docker-dhi`
and `aqua` in the test index denote priority slots; these documents are not
published by Docker or Aqua. Do not export these fixtures as approved exceptions.

Priority: own → Docker DHI advisories → Aqua VEX Hub. A higher-priority affected
or under_investigation statement prevents a lower-priority not_affected decision.
The first applicable statement is selected per package version and vulnerability;
a document about another CVE does not stop fallback.

The Voting workflow resolves this repository ref to a commit SHA at the start
of a run and records source/document hashes. The production integration fetches
only package documents under pkg; it never loads poc/socket/fixtures.
