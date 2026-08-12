# Prompt: Production Engineering Learning Map (Interactive Website)

Use this prompt to generate the full site in one pass, or hand it to another AI/tool to reproduce it.

---

## 1. Goal & audience

Build a single self-contained HTML file titled **"Production Engineering Learning Map"** — a polished, interactive, educational website for a beginner preparing for Production Engineering / Site Reliability Engineering / DevOps / infrastructure interviews. It must teach the fundamentals of **Testing, Containers, CI/CD, Monitoring, and Networking**, and show how these five areas work together to operate a production application. Priority: learning through interaction, not walls of static text — every concept should be something the user clicks, drags, plays, or answers rather than just reads.

## 2. Format & technical constraints

- One self-contained `.html` file: inline `<style>` and inline `<script>`, no external dependencies, no build step, no frameworks (vanilla HTML/CSS/JS only), works offline when opened directly in a browser.
- Wide desktop-first layout (16:9-ish proportions for the hero/lifecycle band), designed to be readable at presentation size, but the page may scroll vertically since there's a lot of content (study-guide style, not a single fixed slide).
- Clean technical-infographic aesthetic: dark navy hero, light neutral canvas background, white cards, one consistent accent color per topic, sufficient color contrast (don't rely on color alone — use icons/labels/borders too), minimal decorative elements.
- All interactive elements must be pure HTML/CSS/JS (no localStorage/sessionStorage), accessible via keyboard (tabindex + Enter/Space where relevant), and gracefully degrade (nothing breaks if a widget is skipped).
- Verify before delivering: balanced HTML tags, valid JS syntax, no duplicate IDs, every JS-referenced ID exists in the markup, every nav link's `href` matches a real section `id`.

## 3. Color system & legend

Define CSS variables for five topic colors (with light bg/border/soft tint variants each), used consistently everywhere (headers, borders, chips, icons):

- Testing — blue `#1d4ed8`
- Containers — teal `#0e7490`
- CI/CD — purple `#6d28d9`
- Monitoring — burnt orange `#c2410c`
- Networking — green `#15803d`

Include a legend bar near the top: color swatches for all 5 topics, a key explaining arrow styles (solid = process flow, dashed = feedback loop, shaded band = environment), and a small icon key. Provide a reusable inline SVG `<symbol>` icon set (test flask, container crate, CI/CD refresh-loop, bar chart, alert bell, server rack, database cylinder, network nodes, code brackets, magnifier) referenced via `<use>` throughout.

## 4. Overall page structure (top to bottom)

1. **Hero header** — title + one-sentence description, dark gradient background.
2. **Sticky top navbar** — links that smooth-scroll to each section (`Lifecycle`, `Testing`, `Containers`, `CI/CD`, `Monitoring`, `Networking`, `Connections`, `On-Call Sim`, `Takeaway`). Use `scroll-behavior: smooth`, `scroll-margin-top` on section targets so the sticky nav doesn't cover headings, and an `IntersectionObserver` scroll-spy that highlights the current section's nav link (color-coded to match that topic).
3. **Legend bar** (see §3).
4. **Central Production Lifecycle panel** — the main visual focus (see §5).
5. **Five topic cards in a responsive grid** (`repeat(auto-fit, minmax(400px,1fr))`) — Testing, Containers, CI/CD, Monitoring, Networking (see §6–§10).
6. **Cross-topic connections panel** (see §11).
7. **On-Call Simulator panel** — capstone activity spanning all 5 topics (see §12).
8. **Final takeaway banner** — dark full-width band with a large centered quote.

## 5. Central Production Lifecycle (SVG diagram)

Left-to-right flow through 8 stages, each an icon + label box: **Developer writes code → Automated testing → CI/CD pipeline → Container image → Production deployment → User network request → Monitoring & alerts → Engineer investigates & improves code**. Connect consecutive stages with solid arrows. Add a large dashed gold feedback arrow curving from the end of the chain back to stages 1–2, labeled "Feedback: monitoring & investigation improve future code and tests" — this is required, not optional.

Group the 8 stages into three labeled background bands so environments are clearly separated: **Development** (stages 1–2), **CI/CD Build & Staging** (stages 3–4), **Production** (stages 5–7, plus stage 8 conceptually closing the loop). Add a small legend under the diagram explaining each band.

## 6. Section 1 — Testing

Content to cover: a **test pyramid** (many unit tests at the base, fewer integration in the middle, few E2E at the top) plus Functional, Regression, Smoke, Performance, Load, Stress testing, Test automation, Test coverage, Test environments. Purpose statement to display verbatim: *"Verify correctness, prevent regressions, and measure behavior before deployment."*

**Interactivity:**
- Render the pyramid as clickable SVG polygons (3 layers). Clicking a layer highlights it (gold stroke) and updates a single detail panel below with that layer's title + one-paragraph explanation — don't show all three descriptions at once.
- All 9 concept terms as **click-to-reveal chips**: term always visible, one-sentence definition hidden until clicked (accordion-style, small chevron rotates on open).
- A **"Pick the Right Test" quiz widget**: a bank of ~7 realistic scenario questions (e.g., "You changed how tax is calculated inside a single pricing function — what catches a bug here fastest?"), each with 3–4 multiple-choice test-type options, one correct answer, instant color-coded feedback (green/red) + a one-line explanation, a running score counter ("Score: X / Y"), and a "Next question →" button that reshuffles through the bank without repeats until exhausted, then reshuffles again.

## 7. Section 2 — Containers

Content: relationship **Dockerfile → Container image → Image registry → Running container**; concepts: application code & dependencies, port mapping, environment variables, volumes, container networking, Docker Compose, health checks, CPU/memory limits, container logs; Kubernetes basics (Pod → Deployment → Service); an example multi-container app (Web container ↔ Database container). Label: *"Consistent, isolated application environments."*

**Interactivity:**
- A **"Container Lifecycle Player"**: the 4-stage flow-strip becomes clickable-through via a Play button that animates each stage (highlight → mark done → advance) with a one-line caption per stage, ending with "▶ Replay".
- All 9 concept chips as click-to-reveal (same pattern as §6).
- A **Memory Limit Simulator**: a range slider (64–512MB) for a container's memory limit, plus a "Simulate load spike" button. On click, generate a random simulated peak usage (300–520MB) and print realistic `kubectl top pod` log output to a dark terminal-style panel — if peak exceeds the limit, show an OOMKilled/restart sequence; otherwise show a healthy result. This should teach CPU/memory limits and health-check/restart behavior tangibly.

## 8. Section 3 — CI/CD

Content: 15-stage pipeline — **Git push/PR → Checkout code → Install dependencies → Lint → Unit tests → Integration tests → Build image → Security scan → Push to registry → Deploy to staging → Smoke test → Approval/quality gate → Deploy to production → Verify health → Notify team** (flag the staging and production stages visually). Definitions: Workflow, Job, Step, Runner, Artifact, Secret. Distinguish Continuous Integration / Continuous Delivery / Continuous Deployment. Deployment strategies: Rolling, Blue-green, Canary, Rollback.

**Interactivity:**
- Give every pipeline stage a unique DOM id. Add a **"Run Pipeline" control**: a dropdown to optionally inject a failure at a specific stage (Lint / Unit tests / Security scan / Approval gate), a Run button that animates stages passing sequentially (~280ms apart, yellow "running" → green "passed"), and — if a failure point is set — stops there (red "failed"), marks every later stage gray/"skipped", and shows a caption explaining that the quality gate blocked an unsafe deploy. Include a Reset button.
- Make the CI / Continuous Delivery / Continuous Deployment cards clickable: each stores how far it goes (`data-through` = stage index), and clicking one highlights (box-shadow) that range of pipeline stages, visually answering "how far does each one actually go?" This click also still reveals its own definition (chip-style flip).
- All Building Blocks and Deployment Strategy chips as click-to-reveal.

## 9. Section 4 — Monitoring

Content: three pillars (Metrics, Logs, Traces — each with a realistic one-line code/log example, e.g. `http_requests_total{status="500"} 42`); system metrics (CPU, memory, disk capacity, disk I/O, network traffic); application metrics (request rate, response time, error rate, throughput); database metrics (query latency, slow queries, active connections, connection-pool saturation, locks); the four golden signals (Latency, Traffic, Errors, Saturation); incident flow (Metric/log/trace → Dashboard → Alert → Engineer investigation → Mitigation → Root-cause analysis → Postmortem → Preventive improvement); health checks, alert thresholds, SLI, SLO, SLA. Label: *"Detect, understand, and respond to production behavior."*

**Interactivity:**
- Pillars and all metric/reliability chips are click-to-reveal (pillars show their example snippet on open).
- An **Error Budget Calculator**: a 5-position range slider (99% / 99.9% / 99.95% / 99.99% / 99.999%) that live-computes and displays allowed downtime per month and per year (based on ~43,829 minutes/month and ~525,949 minutes/year), formatted as human-readable duration (e.g., "43m 50s"). Include a short explanatory line about why teams don't always chase 100%.

## 10. Section 5 — Networking

Content: request path — **User browser → DNS lookup → Public IP → Firewall → Load balancer/reverse proxy → App container → Database → Response returned**; concepts: IP address, Port, DNS, TCP, UDP, HTTP, HTTPS, TLS encryption, Router, Switch, Firewall, Proxy, Reverse proxy, Load balancer, NAT, Subnets, CIDR ranges, Latency, Bandwidth, Packet loss; a troubleshooting toolbox (`ping`, `curl`, `traceroute`, `dig`/`nslookup`, `ss`, `tcpdump`, each with a one-line purpose).

**Interactivity (this section should be the richest):**
- **Request Journey animation**: below the high-level 8-step path, add a "zoom in" widget with a small SVG diagram (Browser box, Server box, a dashed connecting line, a static "DNS resolver" marker between them) and a **moving packet** (an SVG group animated via CSS `transform: translateX()` transitions, ~0.6s ease) that travels between Browser/DNS-marker/Server depending on the current step. Model 12 granular sub-steps across 4 color-coded phases: DNS resolution (query → root → TLD → authoritative → resolved IP), TCP handshake (SYN → SYN-ACK → ACK), TLS handshake (ClientHello → ServerHello+Cert → key exchange), HTTP exchange (encrypted GET/response). Each step updates: packet position, packet label, a phase badge (colored pill: "DNS RESOLUTION" / "TCP HANDSHAKE" / "TLS HANDSHAKE" / "HTTP EXCHANGE"), a step counter, and a caption sentence. Controls: "◀ Prev", "▶ Play all" (auto-advances every ~900ms), "Next ▶".
- All concept chips click-to-reveal.
- A **CIDR/Subnet Calculator**: a text input pre-filled with an example (`192.168.1.0/24`), a Calculate button, and a terminal-style output showing network address, broadcast address, total addresses, and usable hosts — computed client-side with real IP/CIDR math (validate input, handle edge cases like `/31` and `/32`).
- An **Incident Simulator**: give each request-path step a unique id. Provide 6 scenario buttons (DNS failure, Firewall block, LB 502s, App crash, DB saturation, Packet loss). Selecting one: lights up the relevant path step red (hard failure) or orange (degraded/intermittent) with an "✕"/"!" badge, shows a topic-appropriate alert banner (critical vs. warning styling), and enables 6 toolbox buttons (ping/curl/traceroute/dig/ss/tcpdump). Clicking a tool appends realistic simulated terminal output to a dark terminal panel — critically, **not every tool should be equally useful for every scenario** (e.g., `dig`/`tcpdump` are diagnostic for DNS failure, `ss` is inconclusive there) — this teaches real triage judgment. A "Reveal root cause" button shows the fix and highlights (green ring) which 1–2 tools were actually diagnostic for that scenario. Include a Reset button.

## 11. Cross-topic connections

A small radial/pentagon SVG with 5 topic-colored circular nodes (Testing, CI/CD, Containers, Monitoring, Networking) connected by faint lines, next to a list of 9 labeled relationships (colored dot(s) → colored dot(s) + one sentence), covering exactly these: tests run inside CI/CD; CI/CD builds and deploys container images; container ports/networks let services communicate; network traffic generates metrics/logs/traces; monitoring detects failures after deployment; monitoring results lead to new tests and code improvements; failed CI/CD quality gates stop unsafe deployments; health checks connect containers, deployment automation, and monitoring; rollbacks use previous container images when verification fails.

## 12. On-Call Simulator (capstone, all 5 topics)

A multi-round, timed, scored incident-response game (styled after Google SRE's "Wheel of Misfortune" training exercise), placed after the connections panel and before the takeaway.

- A bank of **12 incidents** spanning all 5 topics (2–3 per topic), each with: a topic tag, an alert sentence, exactly 4 plausible action choices (one correct), and an explanation of why the correct action is right.
- "Start Shift" shuffles the 12 into a random order and begins round 1.
- Each round: show a topic-colored tag + alert text, a 15-second countdown progress bar (green → amber under 60% time → red under 30% time), and 4 clickable action buttons.
- Answering: disable all buttons, mark the chosen one green (correct) or red (incorrect, with the true correct one also highlighted green), show the explanation, award points (10 for a correct answer in the first half of the timer, 5 for correct-but-slow, 0 for wrong), update a running score display, reveal a "Continue →" button.
- Timing out with no answer: mark as "escalated", 0 points, still reveal the correct choice and explanation.
- After round 12: show a summary screen — total score out of 120, a grade with an emoji/color threshold (≥80% green "textbook shift", ≥50% amber "solid, rough patch", else red "worth reviewing the sections above"), and a full round-by-round recap list (✓/✗, topic, points). Include a "Start New Shift" button that reshuffles and restarts.

## 13. Final takeaway

Full-width dark banner with this exact quote, centered and prominent: *"Production Engineering combines automated testing, repeatable deployments, reliable runtime environments, network understanding, and continuous monitoring to keep software available, performant, and safe."*

## 14. Universal interaction pattern (apply site-wide)

Every definition chip, pillar, and CI/CD-type card across all 5 sections should follow one consistent click-to-reveal accordion pattern: term/label always visible with a small rotating chevron affordance; definition/detail hidden (`max-height:0; opacity:0`) until clicked or triggered via keyboard (Enter/Space), then expands (`max-height` + `opacity` transition). Wire this with a single generic event-delegated click handler plus a `tabindex`/`role="button"`/`aria-expanded` pass for accessibility, rather than one-off handlers per element. Add a short italic "Tap any card to reveal its definition" hint above the first chip grid in each card.

## 15. Verification checklist before delivering

- Parse the HTML with a strict tag-balance check (accounting for real HTML void elements: `area, base, br, col, embed, hr, img, input, link, meta, param, source, track, wbr`).
- Count `{`/`}` in the `<style>` block to confirm they match.
- Run the extracted `<script>` contents through a JS syntax check (e.g. `node --check`).
- Grep for duplicate `id="..."` attributes — there should be none.
- Confirm every element referenced by `document.getElementById` / `querySelector` in the script actually exists in the markup.
- Confirm every nav `href="#sec-x"` has a matching `id="sec-x"` and vice versa.
- Spot-check any generated/randomized data (e.g., quiz or incident banks) for correct-answer indices that are within bounds of their option arrays.
