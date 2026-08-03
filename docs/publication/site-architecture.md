# Site and content architecture

## Objective

Publish one coherent Outcome Engineering product across positioning, method, reference, examples, knowledge, and tools while preserving one authoritative methodology source.

The existing `outcomeeng/outcome.engineering` repository already supplies a Next.js 15 application, React 19, MDX rendering, Tailwind CSS, blog support, and Vercel deployment. The implementation replaces obsolete methodology content and data inside that stack.

## Route architecture

The `{version}` segment is `next` for a mutable preview and a full immutable release identifier such as `3.2.0` for a released methodology. Preview routes never occupy unversioned canonical aliases.

| Route | Content | Content type | Authority |
| --- | --- | --- | --- |
| `/` | Current-release rendering of the canonical positioning page; it remains the existing homepage while `next` is a preview. | `positioning` | `explanatory` |
| `/method/{version}/overview` | Immutable released positioning page, or the exact `next` positioning preview. | `positioning` | `explanatory` |
| `/method/{version}` | Versioned method-guide index and complete tour. | `guide` | `explanatory` |
| `/method/{version}/abundant-implementation` | Product work under abundant implementation. | `guide` | `explanatory` |
| `/method/{version}/program-logic` | Resources, Activities, Outputs, Outcomes, Impact, and evidence. | `guide` | `explanatory` |
| `/method/{version}/product-truth` | Emergent product truth, authority, learning, and reconciliation. | `guide` | `explanatory` |
| `/method/{version}/durable-map` | Durable declarations, knowledge, deterministic context, and structure. | `guide` | `explanatory` |
| `/method/{version}/changes` | Delta, Change, provenance, lineage, claim, Handoff, and implementation links. | `guide` | `explanatory` |
| `/method/{version}/refinement` | Human framing, slicing, sequencing, discretion, and escalation. | `guide` | `explanatory` |
| `/method/{version}/agent-execution` | Claim, provider-local planning, reconciliation, evidence, Handoff, and integration. | `guide` | `explanatory` |
| `/method/{version}/evidence-and-standing` | Output verification, Outcome evidence, Impact evidence, tier, state, and projection. | `guide` | `explanatory` |
| `/method/{version}/perspectives` | Impact, Outcomes, and Build projections. | `guide` | `explanatory` |
| `/method/{version}/example` | Accessible end-to-end worked example. | `example` | `explanatory` |
| `/method/{version}/adoption` | First product area and first reconciliation cycle. | `guide` | `explanatory` |
| `/method/{version}/sources` | Intellectual sources, influences, and divergences. | `guide` | `informative` |
| `/method/{version}/migration` | Explicit mapping from superseded public terms to current concepts. | `guide` | `informative` |
| `/reference/{version}` | Versioned normative-reference index. | `reference` | `normative` |
| `/reference/{version}/[chapter]` | Rendered authoritative methodology chapter. | `reference` | `normative` |
| `/knowledge/{version}` | Versioned curated-knowledge index. | `knowledge` | `informative` |
| `/knowledge/{version}/[...entry]` | Curated rationale, research, or history. | `knowledge` | `informative` |
| `/tools` | Tool families and installation entry points. | `tool` | `operational` |
| `/tools/[...page]` | Tool-owned operational documentation. | `tool` | `operational` |
| `/blog` | Essays, announcements, and dated publication. | `editorial` | `editorial` |
| `/blog/[slug]` | One dated editorial article. | `editorial` | `editorial` |

When `3.2.0` becomes canonical, the website's current-release pointer makes `/` render the same positioning source as `/method/3.2.0/overview` and emit that immutable route as canonical. The pointer also activates unversioned `/method/...`, `/reference/...`, and `/knowledge/...` redirects to the corresponding `3.2.0` routes. Each destination page emits a canonical link to the full stable versioned URL. A `next` preview keeps self-canonical `/method/next/...`, `/reference/next/...`, and `/knowledge/next/...` URLs and creates no unversioned alias. A Vercel preview may bind its root to the `/method/next/overview` rendering for operator review; the deployed page still emits `https://outcome.engineering/method/next/overview` as canonical until promotion.

The methodology route declaration owns positioning, guide, example, reference, knowledge, and its candidate same-origin aliases. The website route declaration owns `/`, current-release alias activation, tool routes, and editorial routes. The built application merges both declarations and rejects every duplicate path, alias collision, ownership mismatch, or link target absent from the merged inventory.

The primary navigation contains Method, Example, Reference, Tools, and GitHub. Knowledge and Blog appear in a Resources menu. The homepage keeps a persistent methodology-version indicator near the reference entry point.

## Domain boundary

Use `outcome.engineering` as the canonical home for positioning, methodology, examples, and knowledge.

Reserve `docs.outcome.engineering` for detailed tool documentation only if that subdomain is restored. Until then, tool documentation lives under `/tools`. The methodology reference remains on the main domain so readers do not cross a product boundary while moving from explanation to rule.

The existing Mintlify guide and reference tree describes an obsolete methodology generation and duplicates the Next.js content. Remove it after redirects or archive it as a versioned historical snapshot. Do not maintain a second current methodology manuscript there.

## Content type, authority, and metadata

`content_type` and `authority` are separate closed fields. The exporter and website reject values and combinations outside this table.

| Content type | Authority | Visible label | Meaning |
| --- | --- | --- | --- |
| `positioning` | `explanatory` | Method overview | Concise explanation of the canonical method. |
| `guide` | `explanatory` | Method guide | Reader-facing explanation tied to one methodology version. |
| `guide` | `informative` | Method background | Sources or migration context that informs the guide. |
| `example` | `explanatory` | Example | Illustrative application whose governing rules remain in the reference. |
| `reference` | `normative` | Normative reference | Governing methodology text from a pinned source revision. |
| `knowledge` | `informative` | Knowledge | Dated rationale or history with no methodology authority. |
| `tool` | `operational` | Tool documentation | Operational instructions for a named tool version. |
| `editorial` | `editorial` | Editorial | Dated essay or announcement owned by the site publication. |

Proposal and planning artifacts are excluded from the publication export. They receive no public route. A later decision to publish a proposal requires a new content type, authority mapping, and visible label rather than reusing a current category.

All pages carry title, description, content type, authority, canonical URL, source owner, and the closed `indexing` value `index` or `noindex`. Additional metadata depends on ownership:

| Family | Required source metadata |
| --- | --- |
| Positioning, guide, example, reference, knowledge | Methodology version, methodology repository, source commit, source path, declared publication date, and last source update. |
| Tool | Tool name, tool version, source repository, source commit, source path, and compatible methodology versions. |
| Editorial | Author, publication date, site repository, source commit, and source path. |

## Source ownership

### Methodology repository owns

- normative chapters under `versions/{version}`;
- publication manuscripts;
- worked examples;
- knowledge entries selected for publication;
- route titles, descriptions, ordering, and authority labels;
- content publication and update dates;
- diagram semantics: concepts, relationships, direction, labels, text equivalents, and accessible reading order;
- terminology and obsolete-term rules;
- publication version and source provenance.

The methodology export contains only positioning, guide, example, reference, and selected knowledge content.

Diagram specifications live beside their owning manuscripts or in named publication assets. They are exported and digested with the prose. A route declaration maps each diagram specification to every page that renders it.

### Tool repositories own

- command, API, adapter, and installation behavior;
- tool versions and compatibility declarations;
- operational examples tied to a tool release.

The website may own a tool index and presentation. Detailed tool claims identify their tool repository and release rather than a methodology chapter as their source.

### Website editorial source owns

- blog articles;
- release announcements;
- dated editorial metadata.

### Website repository owns

- page layouts and components;
- responsive navigation;
- visual rendering and interaction for exported diagram specifications;
- rendering extensions;
- search indexing;
- redirects;
- analytics and privacy controls;
- deployment configuration;
- presentation tests.

The website cannot alter imported methodology prose during rendering. Method-guide editorial changes begin in the methodology repository and flow through the publication export. Blog editorial changes begin in the website repository.

## Publication export

Create a deterministic export command in the methodology repository. The publication changeset chooses the implementation; its contract is fixed here.

The route declaration lives with this proposal as publication configuration, separate from normative chapter grammar. A versioned `routes.json` maps source paths to public routes, redirects, canonical URLs, titles, descriptions, content types, authority labels, indexability, content dates, diagram specifications, and order. The exporter validates this declaration and produces the manifest below.

The export contains:

```text
publication/
  manifest.json
  positioning/
  guide/
  reference/
  examples/
  knowledge/
  diagrams/
  assets/
```

### Manifest contract

`manifest.json` carries:

```json
{
  "schema": "outcome-engineering-publication/v1",
  "methodology_version": "3.2.0",
  "source_repository": "outcomeeng/methodology",
  "source_commit": "<full commit SHA>",
  "artifact_digest": "sha256:<digest>",
  "files": [
    {
      "path": "reference/principles.md",
      "kind": "reference",
      "media_type": "text/markdown",
      "contributors": [
        {
          "source": "versions/3.2/00-principles.md",
          "source_owner": "outcomeeng/methodology",
          "role": "primary"
        }
      ],
      "sha256": "<file digest>"
    },
    {
      "path": "diagrams/continuous-reconciliation.json",
      "kind": "diagram",
      "media_type": "application/json",
      "contributors": [
        {
          "source": "versions/3.2/publication/diagrams/continuous-reconciliation.json",
          "source_owner": "outcomeeng/methodology",
          "role": "primary"
        }
      ],
      "sha256": "<file digest>"
    }
  ],
  "routes": [
    {
      "source": "versions/3.2/00-principles.md",
      "output": "reference/principles.md",
      "route": "/reference/3.2.0/principles",
      "canonical": "https://outcome.engineering/reference/3.2.0/principles",
      "title": "Principles",
      "description": "<description>",
      "content_type": "reference",
      "authority": "normative",
      "indexing": "index",
      "source_owner": "outcomeeng/methodology",
      "published_on": "<YYYY-MM-DD>",
      "updated_on": "<YYYY-MM-DD>",
      "diagrams": ["diagrams/continuous-reconciliation.json"],
      "order": 10
    }
  ],
  "redirects": [
    {
      "source": "/reference/principles",
      "destination": "/reference/3.2.0/principles",
      "status": 308,
      "applies_to": "canonical:3.2.0",
      "canonical": "https://outcome.engineering/reference/3.2.0/principles"
    }
  ]
}
```

The homepage manuscript produced by M5 lives at `versions/3.2/publication/manuscript/homepage.md`, exports to `positioning/homepage.md`, and appears in `files`. It maps to `/method/next/overview` while the publication version is `next` and to the immutable `/method/{full-version}/overview` route after release. The website current-release pointer additionally renders the selected release at `/`; that rendering emits the immutable full-version route as canonical. Promotion produces a new reviewed release manifest and updates the website pointer atomically with moving aliases. Every full-version positioning route and its bytes remain available indefinitely.

The manifest is generated from the route declaration and source revision. Route and redirect order never depends on filesystem enumeration. The canonical artifact contains no generation timestamp. CI records the generation event outside the exported bytes. `published_on` and `updated_on` are explicit ISO 8601 calendar dates in the route declaration; they are never inferred from filesystem or Git timestamps. The exporter requires `updated_on` to equal or follow `published_on`. `source_owner` is selected from the declared repository owners, and global `source_repository` plus each route's `source` and `source_owner` provide complete source provenance.

Every exported Markdown file, diagram specification, and asset appears once in `files` with `path`, `kind`, `media_type`, `contributors`, and the SHA-256 digest of its exact bytes. `kind` is one of `positioning`, `guide`, `reference`, `example`, `knowledge`, `diagram`, or `asset`. `contributors` is a non-empty array of objects carrying repository-relative `source`, declared `source_owner`, and role `primary`, `included`, or `asset`. Each copied file has exactly one `primary` contributor. A generated file has exactly one `primary` contributor and every additional input as `included` or `asset`; no contributor may be omitted or repeated. Contributor order is bytewise by owner, source, and role. The exporter rejects absent sources, owners outside the declared repository set, output-path collisions, unsupported media types, and files omitted from the manifest. Both fixture and real-export validation apply this exported contract.

`artifact_digest` is SHA-256 over UTF-8 lines of `path`, one NUL byte, the lowercase file digest, and a newline, sorted bytewise by path. The manifest itself is excluded from that list. Routes, redirects, content metadata, file provenance, and diagram mappings are covered by `manifest_sha256`. The manifest uses UTF-8, LF endings, and RFC 8785 JSON canonicalization. `manifest_sha256` is the SHA-256 digest of those exact canonical manifest bytes and lives in the website lock rather than recursively inside the manifest.

### Diagram specification contract

Every exported diagram is RFC 8785 canonical JSON encoded as UTF-8 with `schema: outcome-engineering-diagram/v1` and this closed shape:

```json
{
  "schema": "outcome-engineering-diagram/v1",
  "id": "continuous-reconciliation",
  "title": "Continuous reconciliation",
  "purpose": "Show how learning renews product truth and execution.",
  "scope": "shared",
  "nodes": [
    {"id": "truth", "label": "Product truth", "meaning": "Current durable product conviction."},
    {"id": "evidence", "label": "Evidence", "meaning": "Observation that informs judgment."}
  ],
  "edges": [
    {"id": "learning", "from": "evidence", "to": "truth", "direction": "forward", "label": "informs judgment"}
  ],
  "reading_order": [
    {"kind": "node", "id": "truth"},
    {"kind": "node", "id": "evidence"},
    {"kind": "edge", "id": "learning"}
  ],
  "text_equivalent": "A complete prose explanation of the diagram.",
  "presentation": {
    "groups": [
      {"id": "judgment", "label": "Human judgment", "members": [{"kind": "node", "id": "truth"}]}
    ],
    "emphasis": [{"kind": "node", "id": "truth"}]
  }
}
```

`scope` is `page` or `shared`. Edge `direction` is `forward`, `backward`, `bidirectional`, or `undirected`. Node IDs, edge IDs, and group IDs are each unique in their namespace. Every edge endpoint names a node. Every `reading_order`, group-member, and emphasis reference has the closed shape `{kind: node|edge, id: <ID>}` and resolves in the named namespace. `reading_order` includes every node and edge exactly once. Group and emphasis references add no semantics absent from labels, meanings, relationships, and `text_equivalent`. Each `routes[].diagrams` entry is the exact exported file path of a `kind: diagram` entry in `files`; diagram IDs are never used as route references. A `page` diagram file path appears in exactly one route, while a `shared` diagram file path appears in one or more routes. The exporter validates the whole shape, non-empty strings, route references, and file contributors. The website may choose SVG, HTML, Canvas, or another renderer while preserving this semantic payload.

Methodology-manifest redirect validation accepts only same-origin 308 redirects for durable methodology aliases, requires `source`, `destination`, `applies_to`, and `canonical`, rejects route/redirect source collisions and chains, and proves that every destination and canonical target exists in the same methodology release route set. `next` manifests contain no canonical-release aliases. A canonical release manifest declares every unversioned and compatibility alias applicable to that exact release.

### Website legacy-redirect inventory

The website owns cross-origin, tool, blog, and retired-documentation redirects in `redirects/legacy-routes.json`, separate from the methodology artifact. Its closed schema is `outcome-engineering-website-redirects/v1` and each entry carries:

- `source_origin` and `source_path`;
- `destination_origin` and `destination_path`;
- status `308` for durable movement or `302` for an explicitly temporary bridge;
- `owner`: `methodology`, `tool`, or `website`;
- `reason` and `introduced_in_release`, a stable website release identifier declared before commit;
- optional `retire_after` date for a temporary bridge.

Validation canonicalizes origins, requires HTTPS outside local tests, rejects duplicate sources, route collisions, chains, loops, unknown owners, and expired temporary entries, and checks same-origin destinations against built routes. Release verification checks cross-origin destinations directly and records failures as blockers. Canonical JSON bytes produce `redirect_inventory_sha256`; deployment metadata and the rollback record expose that digest. The website repository reviews, versions, deploys, and restores this inventory independently of the methodology manifest.

### Website route declaration and merged inventory

The website owns `routes/site-routes.json` with schema `outcome-engineering-site-routes/v1`. It declares `/`, tool pages, editorial pages, and any website-owned static page. Each content entry carries route, canonical URL, title, description, content type, authority, `indexing`, source repository, full source commit, source path, owner, and order. Alias entries name their exact destination and current-release selection rule. Tool and editorial metadata must satisfy their family schemas in the content-type table.

At build time, the route compiler loads:

1. every retained full-version methodology manifest;
2. the optional `next` methodology manifest;
3. `routes/site-routes.json`;
4. the current-release pointer;
5. `redirects/legacy-routes.json`.

It emits one `routes.generated.json` containing all content routes, active aliases, external redirects, canonical targets, owners, and indexability. It activates `/` and moving methodology aliases only from the full version named by the current-release pointer. It rejects duplicate routes, route/redirect collisions, aliases to absent versions, source-family metadata violations, links to absent merged routes, and attempts by website content to claim normative authority. `content:check`, the application router, sitemap generator, search indexer, and `check:links` all consume this same generated inventory.

### Link transformation

The exporter resolves and rewrites:

- methodology chapter links to `/reference/{version}/...`;
- publication-guide links to `/method/{version}/...`;
- worked-example links to `/method/{version}/example` or named example routes;
- selected knowledge links to `/knowledge/{version}/...`;
- source-repository links for unexported artifacts;
- asset paths to content-addressed exported assets.

Every source link is validated before export. Every exported link is validated against the manifest after transformation.

### Pinned consumption and release retention

The website stores one immutable lock and generated projection for every full release, plus a replaceable `next` lock while preview work continues:

```text
content/methodology/
  locks/
    next.lock.json
    3.2.0.lock.json
  projections/
    next/
    3.2.0/
  current.json
```

Each version lock carries:

```json
{
  "repository": "outcomeeng/methodology",
  "commit": "<full commit SHA>",
  "methodology_version": "3.2.0",
  "publication_schema": "outcome-engineering-publication/v1",
  "manifest_sha256": "sha256:<digest>",
  "artifact_digest": "sha256:<digest>"
}
```

`current.json` carries:

```json
{
  "schema": "outcome-engineering-current-methodology/v1",
  "methodology_version": "3.2.0",
  "lock_sha256": "sha256:<digest>"
}
```

The lock digest is SHA-256 over the lock's UTF-8 RFC 8785 canonical JSON bytes. `current.json` may name only a retained full-version lock; it never names `next`. Changing `current.json` activates `/`, moving major/minor/unversioned aliases, sitemap priority, and search ranking without changing any full-version projection.

Use this workflow:

1. `pnpm content:sync --version <next-or-full-version>` downloads or checks out that version's pinned methodology revision and runs or retrieves its publication export.
2. Generated content is written to the matching version directory and committed so a Vercel build has no external content dependency.
3. Synchronizing `next` replaces only `next.lock.json` and `projections/next/`.
4. Adding a full release creates a new lock and projection; the command rejects any byte change to an existing full-version lock or projection.
5. `pnpm content:check` regenerates every retained artifact into temporary directories and fails when committed output, canonical manifest bytes, contributor provenance, any per-file digest, `manifest_sha256`, or `artifact_digest` differs.
6. Release promotion adds the full-version lock and projection first, then changes `current.json` and generated aliases in the same reviewable website changeset. It never deletes prior releases.

The committed generated copy is a projection with provenance, never an independently edited source.

The exporter, `content:check`, Next.js production build, Vercel preview verification, and production smoke check all verify the same `manifest_sha256` and `artifact_digest` for every retained version. Preview and production expose the current pointer plus its lock, manifest, and artifact digests in deployment metadata. Exact full-version routes resolve from their retained projections independently of the current pointer.

## Next.js implementation

### Content loader

Create one typed loader for publication content. It validates the manifest and returns:

- route metadata;
- rendered source body;
- table of contents;
- adjacent-page navigation;
- authority label;
- source provenance;
- methodology version.

The loader rejects duplicate routes, unknown authority values, missing source metadata, absent content files, digest mismatches, and links to unknown exported routes.

The return type carries `content_type` and `authority` as separate fields. Validation accepts only the combinations in the closed content-type table above. Each family is decoded through its own metadata schema before rendering, so a normative reference cannot arrive through editorial metadata and a tool page cannot claim methodology authority.

### Page families

Use dedicated layouts:

- **Marketing layout:** homepage and primary positioning.
- **Method layout:** wide narrative pages with diagrams and restrained navigation.
- **Reference layout:** persistent table of contents, direct section links, version badge, source link, and previous/next navigation.
- **Knowledge layout:** date, type, source, non-authoritative label, and related graduated rules.
- **Tool layout:** tool/version selector and operational navigation.
- **Editorial layout:** author, publication date, editorial label, and related method or release links.

### Existing source replacement

The current site hard-codes obsolete concepts across prose and visual data. Replace or remove at least these areas:

- landing-page sections and story copy under `src/components/`;
- `src/lib/story-data.tsx`;
- `src/lib/spec-tree-data.ts`;
- interactive tree state types and labels;
- `content/blog/growing-a-spec-tree.mdx` where it is presented as current method;
- the Mintlify guide and reference tree;
- README and repository instructions that describe two node kinds, lock files, or spec-driven positioning.

Historical essays can remain when clearly dated and labeled as superseded. Current navigation must never route a new reader through obsolete rules without that label.

## Visual system

Keep the current site's strong dark visual identity only where it supports comprehension. Introduce a semantic visual language shared across pages.

### Required diagrams

1. **Program logic:** backward deliberation and forward execution/evidence.
2. **Continuous reconciliation loop:** product truth, Delta, refinement, Change, execution, evidence, learning, and renewed truth.
3. **Authority and learning:** downward authority and upward learning as distinct directions.
4. **Three product perspectives:** Impact, Outcomes, and Build projected from one model.
5. **Change lineage:** one origin refined into several Changes and linked to exact implementation artifacts.
6. **Durable map:** product, decisions, output kinds, outcomes, evidence, implementation, and knowledge.
7. **Human-agent boundary:** judgment and refinement beside delegated execution.

Every diagram has an equivalent text explanation and meaningful accessible labels. Motion adds state transition or causal direction and respects reduced-motion preferences.

### Color semantics

Reserve colors for stable conceptual categories rather than individual pages:

- Impact and externally realized value;
- Outcomes and bets;
- Outputs and durable declarations;
- Activity and execution;
- Resources and expenditure;
- evidence and observation;
- Change coordination;
- provider-owned implementation context.

Color never carries meaning alone.

## Versioning and redirects

### Version policy

The public reference displays a stable methodology version. `next` may be published as a mutable preview with an explicit preview label and canonical source commit. Promotion snapshots one reviewed source commit and artifact digest under an immutable semantic release identifier `vMAJOR.MINOR.PATCH`. It becomes the default only after the readiness gates in [the release plan](release-plan.md) pass.

An exact released route such as `/reference/3.2.0/principles` never changes content or digest. A methodology-content correction creates a new patch release such as `v2.0.1`; a compatible addition creates a minor release; a compatibility-breaking conceptual or structural change creates a major release. Presentation-only website fixes may redeploy the same pinned methodology artifact because its content and canonical routes remain unchanged.

Every `next` route declares `indexing: noindex`. Production responses for `next` content emit `X-Robots-Tag: noindex, nofollow` and matching HTML robots metadata, and the sitemap excludes them. Every Vercel preview deployment applies the same response policy to all routes regardless of their source metadata. Only full immutable release routes with `indexing: index` enter the canonical-release sitemap. The current `/` rendering may be discoverable, while its canonical link points to `/method/{full-version}/overview`.

Unversioned aliases identify the current canonical release. Major aliases such as `/reference/v2/...` identify the latest approved release in that major, and minor aliases such as `/reference/v2.0/...` identify the latest approved patch in that minor. Every alias redirect is version-controlled in the website and recorded in the publication manifest for the release that moves it. Exact full-version routes remain available indefinitely.

Keep stable versioned reference URLs available after a new methodology version becomes canonical:

```text
/method/v1/...
/method/v2/...
/method/3.2.0/...
/reference/v1/...
/reference/v2/...
/reference/3.2.0/...
/knowledge/v1/...
/knowledge/v2/...
/knowledge/3.2.0/...
```

The unversioned `/method/...`, `/reference/...`, and `/knowledge/...` routes redirect to the current canonical version and emit that stable versioned URL as canonical. Preview content remains under `/method/next/...`, `/reference/next/...`, and `/knowledge/next/...` and never receives an unversioned alias.

### Redirect policy

Map every indexed legacy URL to one of:

- its current conceptual replacement;
- a versioned historical page;
- an explanatory migration page when no direct replacement exists.

Avoid silent redirects from an obsolete concept to a semantically different current concept.

## Search and discovery

Search results display content authority and methodology version. Ranking prefers:

1. current normative reference;
2. current method guide;
3. current examples;
4. current tool documentation;
5. knowledge and historical content.

Index headings and explicit aliases. Do not use obsolete terminology as invisible current-page keyword stuffing. A migration glossary can map old terms to current concepts explicitly.

## Quality gates

### Required website commands

The website repository defines these scripts as one public verification contract:

| Command | Contract |
| --- | --- |
| `pnpm content:sync` | Materialize the publication artifact for the pinned methodology commit. |
| `pnpm content:check` | Reproduce the artifact and reject byte, route, link, metadata, or digest drift. |
| `pnpm validate:repo` | Run `spx validation all` exactly. |
| `pnpm typecheck` | Run `tsc --noEmit`. |
| `pnpm lint` | Run ESLint over the application and repository scripts. |
| `pnpm test:unit` | Run Vitest unit and component tests. |
| `pnpm build` | Produce the Next.js production build. |
| `pnpm check:links` | Crawl the built route declaration and reject unresolved internal routes, assets, anchors, and canonical targets. |
| `pnpm test:e2e` | Run Playwright against the production build. |
| `pnpm test:a11y` | Run automated Playwright accessibility scenarios with `@axe-core/playwright`. |
| `pnpm verify` | Run every blocking command above in a documented deterministic order. |

The implementation adds `@playwright/test` and `@axe-core/playwright`; Vitest remains the unit and component runner. The link checker is a repository-owned script over the exported route declaration and built output. Any replacement tool must preserve the command names and evidence contract so local, CI, preview, and release checks remain identical.

### Content gates

- Every published claim has an owned source file.
- Every page declares authority and version.
- Every route declares `index` or `noindex`; preview routes are always `noindex`.
- Every normative page maps to one methodology source path and commit.
- Internal links resolve after route transformation.
- External links are checked on a scheduled, non-blocking cadence and classified by source criticality.
- Current pages contain no prohibited obsolete terms except in labeled historical or migration contexts.
- Program-logic terms follow the reconciled definitions.
- Change terminology follows the graduated coordination model.

### Application gates

- `pnpm validate:repo` succeeds.
- `pnpm content:check` succeeds against the committed lock and generated content.
- `pnpm typecheck` succeeds.
- `pnpm lint` succeeds.
- `pnpm build` succeeds.
- unit tests for manifest parsing, link transformation, route uniqueness, and digest validation.
- component tests for authority labels, version badges, navigation, and source provenance.
- Playwright smoke tests for every page family.
- responsive checks at phone, tablet, desktop, and wide desktop sizes.
- keyboard navigation and focus order.
- automated accessibility scan plus manual diagram and heading review.
- no unexpected horizontal overflow.
- reduced-motion behavior.
- canonical metadata, sitemap, robots policy, Open Graph, and structured data.
- immutable positioning routes and root canonical metadata agree with the selected full release.
- `next` and Vercel-preview pages emit `noindex, nofollow` and never enter the canonical-release sitemap.
- broken-link scan over the built site.

### CI and deployment gates

Every pull request blocks on `validate:repo`, `content:check`, `typecheck`, `lint`, `test:unit`, `build`, `check:links`, `test:e2e`, and `test:a11y`. Tests assert robots headers, metadata, and sitemap exclusion for `next` and Vercel-preview renderings. The jobs may execute in parallel after content synchronization, while their inputs remain the same committed lock and generated artifact.

A Vercel preview is reviewable only after the blocking checks pass and its deployment metadata reports the reviewed `manifest_sha256`, `artifact_digest`, and `redirect_inventory_sha256`. Production promotion requires the same checks on the integration commit, operator approval of the named preview, and exact digest matches between the preview, integration commit, and production deployment. A post-deployment smoke run verifies the canonical routes, redirects, source labels, and digests before the prior deployment is released as the rollback target.

Automated accessibility and human accessibility review produce separate evidence. `pnpm test:a11y` is the blocking automated CI command. Before production promotion, an assigned human accessibility reviewer records keyboard flow, focus order, heading structure, diagram text equivalence, color-independent meaning, reduced motion, zoom, and screen-reader observations in `docs/release-evidence/accessibility/{methodology-version}.md` in the website repository. The release owner links that approved record from the production checklist; a missing or rejected record blocks promotion.

### Publication gates

- Methodology source commit has passed its own review and merge lifecycle.
- Website pin resolves to that exact commit.
- Preview deployment shows the expected manifest digest.
- Preview deployment shows the expected website redirect-inventory digest.
- Human accessibility review is approved and recorded for the release candidate.
- Operator approves the homepage argument, method sequence, visual semantics, and worked example.
- Production smoke tests pass after deployment.

## Rollback evidence schemas

Rollback evidence uses four files under `docs/release-evidence/rollback/{release}/`:

| File | Schema |
| --- | --- |
| `dns.json` | `outcome-engineering-dns-snapshot/v1` |
| `site.json` | `outcome-engineering-route-snapshot/v1` |
| `external-docs.json` | `outcome-engineering-external-docs-snapshot/v1` |
| `record.json` | `outcome-engineering-rollback-record/v1` |

Every file is UTF-8 RFC 8785 canonical JSON with no byte-order mark or trailing newline. All source strings are Unicode NFC before canonicalization. Digests use lowercase hexadecimal with the `sha256:` prefix over the exact canonical bytes.

The closed payload shapes are:

```json
{
  "schema": "outcome-engineering-dns-snapshot/v1",
  "provider": "<provider>",
  "records": [
    {"name": "outcome.engineering.", "type": "A", "ttl": 300, "values": ["<RDATA>"]}
  ]
}
```

```json
{
  "schema": "outcome-engineering-route-snapshot/v1",
  "origin": "https://outcome.engineering",
  "deployment_id": "<immutable provider deployment ID>",
  "routes": [
    {
      "path": "/",
      "status": 200,
      "final_url": "https://outcome.engineering/",
      "canonical_url": "https://outcome.engineering/",
      "robots": "index,follow",
      "content_type": "text/html",
      "body_sha256": "sha256:<digest>"
    }
  ]
}
```

`external-docs.json` uses the same route-entry shape with schema `outcome-engineering-external-docs-snapshot/v1` and adds top-level `provider`, `origin`, and `deployment_id`.

```json
{
  "schema": "outcome-engineering-rollback-record/v1",
  "release": "3.2.0",
  "captured_at": "<UTC ISO-8601 instant>",
  "capture_operator": "<operator identity>",
  "prior_deployments": {"site": "<ID>", "external_docs": "<ID>"},
  "component_digests": {
    "dns": "sha256:<digest>",
    "external_docs": "sha256:<digest>",
    "redirects": "sha256:<digest>",
    "site": "sha256:<digest>"
  },
  "candidate_digests": {
    "manifest": "sha256:<digest>",
    "artifact": "sha256:<digest>",
    "redirects": "sha256:<digest>"
  },
  "prior_state_sha256": "sha256:<digest>"
}
```

`dns.json` contains provider and records. Each record contains normalized owner name, uppercase type, integer TTL, and a bytewise-sorted value array. Owner names use lowercase IDNA ASCII with a trailing dot. Provider-exported RDATA is trimmed and preserved; domain-name RDATA receives the same lowercase fully qualified normalization. Records sort bytewise by owner name and type, and duplicate owner/type pairs are rejected.

`site.json` contains origin, immutable deployment identity, and route entries. `external-docs.json` adds provider and deployment identity and otherwise uses the same route-entry shape. Each route entry contains request path, integer status, final URL, canonical URL or `null`, robots value or `null`, lowercase content type without parameters, and SHA-256 of the exact response body bytes. URL origins use lowercase scheme and IDNA host with default ports removed; paths and queries use WHATWG URL serialization. Duplicate request paths are rejected, and route entries sort bytewise by request path. Capture uses `GET`, no cookies, no authentication, and `Accept-Encoding: identity`.

For `canonical_url`, capture all final-response HTTP `Link` values with `rel=canonical` and all HTML `<link rel="canonical">` values, resolve relative values against `final_url`, and normalize them through the same WHATWG serialization. Zero values produce `null`; one distinct normalized value produces that value; multiple distinct values fail capture.

For `robots`, capture final-response `X-Robots-Tag` directives with no user-agent prefix or the `*` prefix and HTML `<meta name="robots">` directives. Directive names and values become lowercase, optional whitespace is trimmed, `none` expands to `noindex` and `nofollow`, and `all` expands to `index` and `follow`. Duplicate directives collapse, contradictory `index`/`noindex` or `follow`/`nofollow` pairs fail capture, and remaining directives sort bytewise and join with commas and no spaces. Zero applicable directives produce `null`. The capture rejects malformed directives and therefore never resolves disagreement by implementation-specific precedence.

The exact canonical bytes of `redirects/legacy-routes.json` supply `redirect_inventory_sha256`. `record.json` carries the four component digests, prior deployment identities, new candidate publication digests, capture operator, and capture time. `prior_state_sha256` is SHA-256 over these UTF-8 bytes in this fixed order:

```text
dns\0<dns_sha256>\n
external-docs\0<external_docs_sha256>\n
redirects\0<redirect_inventory_sha256>\n
site\0<site_sha256>\n
```

The angle-bracket expressions are replaced by complete prefixed digest strings, `\0` denotes one NUL byte, and `\n` denotes one LF byte. A restored state must reproduce every component digest and `prior_state_sha256`. Dynamic content that cannot reproduce exact bytes requires an explicit operator-approved exception recorded before cutover with replacement evidence and acceptance rules; absent that decision, the release is blocked.

## Analytics

Measure whether the exposition works while minimizing collection:

- homepage to method-guide continuation;
- method-guide completion by chapter;
- example entry and completion;
- reference transitions from explanatory pages;
- tool-installation entry;
- search queries that return no useful result;
- obsolete-term searches indicating migration needs.

Avoid treating page views as Outcome evidence by themselves. Define the desired reader and adoption Outcomes before choosing measures.

## Current deployment observations

Observed on 2026-07-24:

- `outcome.engineering` serves the Next.js application through Vercel.
- the public site presents the two-kind enabler/outcome model, lock files, and old node states;
- the source repository contains nineteen Mintlify MDX pages describing that earlier model;
- `docs.outcome.engineering` has no DNS answer;
- the local primary checkout was on `refactor/xiperlabs-architecture` with uncommitted `.claude`, workflow, and `.spx` changes;
- `origin/main` was commit `5dfcee5527268b8e7f5c4fb3c731c442222a346f`.

These observations are pickup context rather than durable assumptions. The resumed website session verifies remote, worktree, deployment, and DNS state before acting. It creates a fresh worktree or otherwise isolates publication changes from the existing modified checkout.
