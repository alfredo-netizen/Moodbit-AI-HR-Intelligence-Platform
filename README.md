<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <title>Moodbit – AI HR Intelligence on Google Cloud</title>
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <meta name="description" content="Moodbit is an AI-powered HR intelligence platform built on Google Cloud, using Vertex AI, BigQuery and Cloud Run to deliver secure, real-time HR insights." />
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet" />
  <style>
    :root {
      --bg-main: #f5f7fb;
      --bg-card: #ffffff;
      --border-subtle: #e2e6f0;
      --accent: #2563eb;
      --accent-soft: rgba(37, 99, 235, 0.08);
      --accent-strong: #4f46e5;
      --text-main: #0f172a;
      --text-muted: #64748b;
      --shadow-soft: 0 18px 45px rgba(15, 23, 42, 0.08);
      --shadow-strong: 0 20px 60px rgba(37, 99, 235, 0.35);
      --radius-lg: 20px;
      --radius-xl: 32px;
      --radius-pill: 999px;
      --gradient-accent: linear-gradient(135deg, #2563eb, #4f46e5);
      --gradient-soft: radial-gradient(circle at top left, #e0edff 0, #f5f7fb 40%, #ffffff 100%);
    }

    * {
      box-sizing: border-box;
    }

    html, body {
      margin: 0;
      padding: 0;
      width: 100%;
      height: 100%;
      scroll-behavior: smooth;
    }

    body {
      font-family: "Inter", system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
      background: var(--gradient-soft);
      color: var(--text-main);
      line-height: 1.6;
      position: relative;
      overflow-x: hidden;
    }

    a {
      color: var(--accent);
      text-decoration: none;
    }

    a:hover {
      text-decoration: underline;
    }

    /* ==== Cinematic animated background ==== */

    .bg-canvas {
      position: fixed;
      inset: 0;
      z-index: -1;
    }

    .bg-overlay {
      position: fixed;
      inset: 0;
      background:
        radial-gradient(circle at top left, rgba(59, 130, 246, 0.16), transparent 55%),
        radial-gradient(circle at bottom right, rgba(79, 70, 229, 0.18), transparent 60%);
      mix-blend-mode: screen;
      opacity: 0.9;
      pointer-events: none;
      z-index: -1;
    }

    .page {
      max-width: 1120px;
      margin: 0 auto;
      padding: 32px 20px 60px;
      position: relative;
      z-index: 1;
    }

    /* ==== Header ==== */

    header {
      display: flex;
      align-items: center;
      justify-content: space-between;
      gap: 20px;
      margin-bottom: 32px;
    }

    .logo-mark {
      display: inline-flex;
      align-items: center;
      gap: 10px;
    }

    .logo-icon {
      width: 42px;
      height: 42px;
      border-radius: 18px;
      background:
        radial-gradient(circle at 30% 20%, #60a5fa, #2563eb);
      box-shadow: 0 14px 35px rgba(37, 99, 235, 0.7);
      position: relative;
      overflow: hidden;
    }

    .logo-icon::before,
    .logo-icon::after {
      content: "";
      position: absolute;
      border-radius: 999px;
      border: 2px solid rgba(255, 255, 255, 0.9);
      filter: drop-shadow(0 0 10px rgba(255, 255, 255, 0.8));
    }

    .logo-icon::before {
      width: 24px;
      height: 24px;
      top: 14px;
      left: -8px;
    }

    .logo-icon::after {
      width: 30px;
      height: 30px;
      top: -6px;
      right: -4px;
    }

    .logo-text-main {
      font-weight: 700;
      font-size: 1.15rem;
      letter-spacing: 0.03em;
    }

    .logo-text-sub {
      font-size: 0.75rem;
      color: var(--text-muted);
      text-transform: uppercase;
      letter-spacing: 0.2em;
    }

    .tag-cloud {
      display: flex;
      flex-wrap: wrap;
      gap: 10px;
      justify-content: flex-end;
      font-size: 0.8rem;
    }

    .chip {
      padding: 6px 12px;
      border-radius: var(--radius-pill);
      border: 1px solid rgba(148, 163, 184, 0.5);
      background: rgba(255, 255, 255, 0.7);
      color: var(--text-muted);
      display: inline-flex;
      align-items: center;
      gap: 6px;
      backdrop-filter: blur(10px);
      box-shadow: 0 10px 25px rgba(15, 23, 42, 0.03);
    }

    .chip--accent {
      border-color: transparent;
      background: var(--gradient-accent);
      color: white;
      box-shadow: var(--shadow-strong);
      position: relative;
      overflow: hidden;
    }

    .chip--accent::after {
      content: "";
      position: absolute;
      inset: 0;
      background: linear-gradient(120deg, transparent 0%, rgba(255, 255, 255, 0.55) 35%, transparent 70%);
      opacity: 0;
      transform: translateX(-40%);
      pointer-events: none;
      animation: chipShine 4s ease-in-out infinite;
    }

    .chip-dot {
      width: 7px;
      height: 7px;
      background: #22c55e;
      border-radius: 999px;
      box-shadow: 0 0 6px rgba(34, 197, 94, 0.9);
    }

    @keyframes chipShine {
      0%, 60% {
        opacity: 0;
        transform: translateX(-40%);
      }
      75% {
        opacity: 1;
        transform: translateX(60%);
      }
      100% {
        opacity: 0;
        transform: translateX(80%);
      }
    }

    /* ==== Hero ==== */

    .hero {
      background: rgba(255, 255, 255, 0.98);
      border-radius: var(--radius-xl);
      box-shadow: var(--shadow-soft);
      padding: 32px 28px;
      margin-bottom: 32px;
      position: relative;
      overflow: hidden;
      border: 1px solid rgba(148, 163, 184, 0.45);
      backdrop-filter: blur(6px);
    }

    .hero::before {
      content: "";
      position: absolute;
      inset: -1px;
      border-radius: inherit;
      padding: 1px;
      background: conic-gradient(from 160deg, rgba(37, 99, 235, 0.2), transparent 25%, rgba(94, 234, 212, 0.24), transparent 60%, rgba(129, 140, 248, 0.4));
      -webkit-mask: linear-gradient(#000 0 0) content-box, linear-gradient(#000 0 0);
      -webkit-mask-composite: xor;
              mask-composite: exclude;
      opacity: 0.7;
      pointer-events: none;
    }

    .hero-grid {
      display: grid;
      grid-template-columns: minmax(0, 1.6fr) minmax(0, 1.2fr);
      gap: 28px;
      position: relative;
      z-index: 1;
    }

    .hero-eyebrow {
      display: inline-flex;
      align-items: center;
      gap: 8px;
      font-size: 0.78rem;
      padding: 4px 10px;
      border-radius: var(--radius-pill);
      background: rgba(15, 23, 42, 0.03);
      border: 1px solid rgba(148, 163, 184, 0.5);
      margin-bottom: 12px;
      color: var(--text-muted);
    }

    .hero-eyebrow .chip-dot {
      box-shadow: 0 0 10px rgba(34, 197, 94, 0.8);
    }

    .hero-title {
      font-size: 2.1rem;
      line-height: 1.2;
      margin: 0 0 10px;
      letter-spacing: -0.03em;
    }

    .hero-title span {
      background: var(--gradient-accent);
      -webkit-background-clip: text;
      color: transparent;
    }

    .hero-subtitle {
      font-size: 0.98rem;
      color: var(--text-muted);
      max-width: 520px;
      margin-bottom: 18px;
    }

    .hero-list {
      list-style: none;
      padding: 0;
      margin: 0 0 18px;
      color: var(--text-muted);
      font-size: 0.9rem;
    }

    .hero-list li {
      display: flex;
      align-items: flex-start;
      gap: 8px;
      margin-bottom: 6px;
    }

    .hero-list-dot {
      margin-top: 8px;
      width: 6px;
      height: 6px;
      border-radius: 999px;
      background: #38bdf8;
      flex-shrink: 0;
      box-shadow: 0 0 8px rgba(56, 189, 248, 0.9);
    }

    .hero-cta-row {
      display: flex;
      flex-wrap: wrap;
      gap: 12px;
      align-items: center;
    }

    .hero-cta-primary {
      padding: 10px 20px;
      border-radius: var(--radius-pill);
      background: var(--gradient-accent);
      color: white;
      font-size: 0.95rem;
      font-weight: 500;
      border: none;
      cursor: pointer;
      box-shadow: var(--shadow-strong);
      position: relative;
      overflow: hidden;
      transform: translateY(0);
      transition: transform 0.18s ease-out, box-shadow 0.18s ease-out;
    }

    .hero-cta-primary::after {
      content: "";
      position: absolute;
      inset: 0;
      background: linear-gradient(120deg, transparent 0%, rgba(255, 255, 255, 0.55) 35%, transparent 70%);
      opacity: 0;
      transform: translateX(-40%);
      pointer-events: none;
    }

    .hero-cta-primary:hover {
      transform: translateY(-2px);
      box-shadow: 0 20px 55px rgba(37, 99, 235, 0.55);
    }

    .hero-cta-primary:hover::after {
      animation: buttonShine 0.8s ease-out forwards;
    }

    @keyframes buttonShine {
      0% {
        opacity: 0;
        transform: translateX(-40%);
      }
      40% {
        opacity: 1;
      }
      100% {
        opacity: 0;
        transform: translateX(60%);
      }
    }

    .hero-cta-meta {
      font-size: 0.8rem;
      color: var(--text-muted);
    }

    .hero-cta-meta strong {
      color: var(--text-main);
    }

    /* ==== Architecture card ==== */

    .hero-architecture {
      background: rgba(15, 23, 42, 0.02);
      border-radius: 22px;
      padding: 18px 18px 14px;
      border: 1px solid rgba(148, 163, 184, 0.6);
      position: relative;
      overflow: hidden;
      box-shadow: 0 18px 45px rgba(15, 23, 42, 0.08);
    }

    .hero-architecture::before {
      content: "";
      position: absolute;
      inset: -40%;
      background:
        radial-gradient(circle at 20% 0, rgba(125, 211, 252, 0.35), transparent 70%),
        radial-gradient(circle at 100% 100%, rgba(129, 140, 248, 0.35), transparent 70%);
      opacity: 0.65;
      pointer-events: none;
    }

    .hero-architecture-inner {
      position: relative;
      z-index: 1;
    }

    .hero-architecture-header {
      display: flex;
      align-items: center;
      justify-content: space-between;
      margin-bottom: 14px;
    }

    .hero-architecture-title {
      font-size: 0.9rem;
      font-weight: 600;
    }

    .hero-architecture-badges {
      display: flex;
      gap: 6px;
      flex-wrap: wrap;
    }

    .hero-architecture-badges span {
      font-size: 0.68rem;
      padding: 3px 8px;
      border-radius: var(--radius-pill);
      background: rgba(15, 23, 42, 0.8);
      color: #e5e7eb;
      box-shadow: 0 0 0 1px rgba(148, 163, 184, 0.5);
    }

    .cloud-graph {
      display: grid;
      grid-template-columns: repeat(3, minmax(0, 1fr));
      gap: 8px;
      font-size: 0.78rem;
    }

    .cloud-node {
      background: rgba(255, 255, 255, 0.95);
      border-radius: 14px;
      padding: 8px 9px;
      border: 1px solid rgba(148, 163, 184, 0.7);
      display: flex;
      flex-direction: column;
      gap: 3px;
      position: relative;
      overflow: hidden;
      box-shadow: 0 10px 24px rgba(15, 23, 42, 0.12);
    }

    .cloud-node::after {
      content: "";
      position: absolute;
      inset: 0;
      background: linear-gradient(135deg, rgba(59, 130, 246, 0.09), transparent 40%);
      mix-blend-mode: soft-light;
      pointer-events: none;
    }

    .cloud-node-label {
      font-size: 0.74rem;
      font-weight: 600;
      color: #0f172a;
    }

    .cloud-node-desc {
      font-size: 0.7rem;
      color: var(--text-muted);
    }

    .cloud-node-pill {
      display: inline-flex;
      align-items: center;
      gap: 4px;
      font-size: 0.68rem;
      padding: 2px 6px;
      border-radius: var(--radius-pill);
      background: var(--accent-soft);
      color: #1d4ed8;
      margin-top: 2px;
      text-transform: uppercase;
      letter-spacing: 0.08em;
    }

    .cloud-node-pill-dot {
      width: 6px;
      height: 6px;
      border-radius: 999px;
      background: #60a5fa;
      box-shadow: 0 0 6px rgba(96, 165, 250, 0.9);
    }

    /* ==== Sections & cards ==== */

    .section {
      margin-bottom: 26px;
      background: rgba(255, 255, 255, 0.97);
      border-radius: var(--radius-lg);
      padding: 20px 22px;
      border: 1px solid var(--border-subtle);
      box-shadow: 0 10px 30px rgba(15, 23, 42, 0.06);
      backdrop-filter: blur(4px);
    }

    .section-header {
      display: flex;
      justify-content: space-between;
      align-items: baseline;
      gap: 12px;
      margin-bottom: 10px;
    }

    .section-title {
      font-size: 1.02rem;
      font-weight: 600;
      display: flex;
      align-items: center;
      gap: 8px;
    }

    .section-pill {
      font-size: 0.68rem;
      padding: 3px 8px;
      border-radius: var(--radius-pill);
      background: rgba(37, 99, 235, 0.06);
      color: #1d4ed8;
      text-transform: uppercase;
      letter-spacing: 0.09em;
    }

    .section-kicker {
      font-size: 0.8rem;
      color: var(--text-muted);
    }

    .section p {
      font-size: 0.9rem;
      color: var(--text-muted);
      margin: 0 0 6px;
    }

    .section strong {
      color: var(--text-main);
      font-weight: 600;
    }

    .section-grid {
      display: grid;
      grid-template-columns: repeat(3, minmax(0, 1fr));
      gap: 14px;
      margin-top: 12px;
    }

    .section-card {
      border-radius: 16px;
      padding: 10px 12px;
      background: #f8fafc;
      border: 1px solid rgba(148, 163, 184, 0.6);
      font-size: 0.84rem;
      color: var(--text-muted);
      box-shadow: 0 12px 26px rgba(15, 23, 42, 0.08);
      position: relative;
      overflow: hidden;
      transition:
        transform 0.2s ease-out,
        box-shadow 0.2s ease-out,
        border-color 0.2s ease-out;
    }

    .section-card::after {
      content: "";
      position: absolute;
      inset: 0;
      background: radial-gradient(circle at top left, rgba(59, 130, 246, 0.12), transparent 55%);
      opacity: 0;
      pointer-events: none;
      transition: opacity 0.2s ease-out;
    }

    .section-card:hover {
      transform: translateY(-4px) translateZ(0);
      box-shadow: 0 18px 40px rgba(15, 23, 42, 0.16);
      border-color: rgba(59, 130, 246, 0.7);
    }

    .section-card:hover::after {
      opacity: 1;
    }

    .section-card h4 {
      font-size: 0.85rem;
      margin: 0 0 4px;
      color: var(--text-main);
    }

    .meta-row {
      display: flex;
      flex-wrap: wrap;
      gap: 10px;
      margin-top: 8px;
      font-size: 0.78rem;
      color: var(--text-muted);
    }

    .meta-row span {
      padding: 4px 9px;
      border-radius: var(--radius-pill);
      background: #f1f5f9;
      border: 1px solid rgba(148, 163, 184, 0.6);
    }

    footer {
      margin-top: 24px;
      font-size: 0.78rem;
      color: var(--text-muted);
      text-align: center;
    }

    footer a {
      color: var(--accent);
      font-weight: 500;
    }

    /* ==== Scroll reveal animations ==== */

    .reveal {
      opacity: 0;
      transform: translateY(18px);
      transition:
        opacity 0.65s cubic-bezier(0.19, 1, 0.22, 1),
        transform 0.65s cubic-bezier(0.19, 1, 0.22, 1);
      will-change: opacity, transform;
    }

    .reveal.visible {
      opacity: 1;
      transform: translateY(0);
    }

    /* Slight delay variants */
    .reveal.delay-1 {
      transition-delay: 0.08s;
    }
    .reveal.delay-2 {
      transition-delay: 0.16s;
    }
    .reveal.delay-3 {
      transition-delay: 0.24s;
    }

    /* ==== Responsive ==== */

    @media (max-width: 880px) {
      .hero-grid {
        grid-template-columns: minmax(0, 1fr);
      }
      .section-grid {
        grid-template-columns: minmax(0, 1fr);
      }
      header {
        flex-direction: column;
        align-items: flex-start;
      }
      .tag-cloud {
        justify-content: flex-start;
      }
    }

    @media (max-width: 600px) {
      .page {
        padding: 20px 16px 40px;
      }
      .hero {
        padding: 22px 18px;
        border-radius: 22px;
      }
      .hero-title {
        font-size: 1.6rem;
      }
    }
  </style>
</head>
<body>
  <!-- animated background -->
  <canvas class="bg-canvas" id="bg-canvas"></canvas>
  <div class="bg-overlay"></div>

  <div class="page">
    <header class="reveal visible">
      <div class="logo-mark">
        <div class="logo-icon"></div>
        <div>
          <div class="logo-text-main">Moodbit</div>
          <div class="logo-text-sub">AI HR INTELLIGENCE</div>
        </div>
      </div>
      <div class="tag-cloud">
        <span class="chip chip--accent">
          <span class="chip-dot"></span>
          Built on Google Cloud
        </span>
        <span class="chip">GenAI HR Agents</span>
        <span class="chip">Cognitive Search</span>
        <span class="chip">People Analytics</span>
      </div>
    </header>

    <main>
      <!-- HERO -->
      <section class="hero reveal">
        <div class="hero-grid">
          <div>
            <div class="hero-eyebrow">
              <span class="chip-dot"></span>
              Case study · Solution qualification for Google Cloud
            </div>
            <h1 class="hero-title">
              AI-driven HR Intelligence
              <span>powered by Google Cloud</span>
            </h1>
            <p class="hero-subtitle">
              Moodbit is an AI-powered HR intelligence platform that helps organizations streamline HR operations, understand workforce sentiment, and make data-driven decisions through secure GenAI agents and Cognitive Search.
            </p>
            <ul class="hero-list">
              <li>
                <div class="hero-list-dot"></div>
                <span><strong>AI agents for HR:</strong> automate FAQs, policies, workflows and employee support on top of existing HR tools.</span>
              </li>
              <li>
                <div class="hero-list-dot"></div>
                <span><strong>Cognitive Search:</strong> conversational access to HR policies, documentation, and employee data in real time.</span>
              </li>
              <li>
                <div class="hero-list-dot"></div>
                <span><strong>Google Cloud native:</strong> built with Vertex AI, BigQuery and Cloud Run for scalability, security and compliance.</span>
              </li>
            </ul>
            <div class="hero-cta-row">
              <a href="https://mymoodbit.cc" class="hero-cta-primary" target="_blank" rel="noopener">
                Visit Moodbit Website
              </a>
              <div class="hero-cta-meta">
                Contact: <strong>alfredo@mymoodbit.cc</strong><br />
                Deployment: <strong>United States &amp; global customers</strong>
              </div>
            </div>
          </div>

          <!-- ARCHITECTURE SUMMARY -->
          <aside class="hero-architecture reveal delay-1">
            <div class="hero-architecture-inner">
              <div class="hero-architecture-header">
                <div class="hero-architecture-title">Google Cloud Solution Architecture</div>
                <div class="hero-architecture-badges">
                  <span>Vertex AI</span>
                  <span>BigQuery</span>
                  <span>Cloud Run</span>
                </div>
              </div>
              <div class="cloud-graph">
                <div class="cloud-node">
                  <div class="cloud-node-label">HR Data Layer</div>
                  <div class="cloud-node-desc">
                    HRIS, payroll, engagement platforms and document repositories are connected through secure APIs and batch pipelines into Google Cloud Storage and BigQuery.
                  </div>
                  <div class="cloud-node-pill">
                    <span class="cloud-node-pill-dot"></span>
                    DATA INGESTION
                  </div>
                </div>
                <div class="cloud-node">
                  <div class="cloud-node-label">AI &amp; Search Layer</div>
                  <div class="cloud-node-desc">
                    Vertex AI hosts LLMs and embeddings for Moodbit’s GenAI agents and Cognitive Search, providing semantic understanding over structured and unstructured HR data.
                  </div>
                  <div class="cloud-node-pill">
                    <span class="cloud-node-pill-dot"></span>
                    INTELLIGENCE
                  </div>
                </div>
                <div class="cloud-node">
                  <div class="cloud-node-label">Experience Layer</div>
                  <div class="cloud-node-desc">
                    Employee and HR portals run on Cloud Run and Cloud Load Balancing, exposing secure APIs, chat interfaces and dashboards for self-service insights and actions.
                  </div>
                  <div class="cloud-node-pill">
                    <span class="cloud-node-pill-dot"></span>
                    EXPERIENCE
                  </div>
                </div>
              </div>
            </div>
          </aside>
        </div>
      </section>

      <!-- SECTION: OVERVIEW -->
      <section class="section reveal">
        <div class="section-header">
          <h2 class="section-title">
            Solution overview
            <span class="section-pill">HR · AI · ANALYTICS</span>
          </h2>
          <div class="section-kicker">
            We manage HR data. You focus on your people.
          </div>
        </div>
        <p>
          Moodbit centralizes HR data and makes it instantly usable through an AI-first experience. HR leaders and managers interact with conversational agents to explore policies, identify risk, and understand employee sentiment, while employees receive personalized, always-on support. GenAI-powered Cognitive Search allows users to ask natural-language questions across policies, contracts, surveys, performance reviews and system logs.
        </p>
        <p>
          The platform is designed for modern, distributed organizations that need actionable insight rather than static dashboards. By unifying HR data and surfacing real-time recommendations, Moodbit improves workforce planning, engagement and retention while reducing the manual workload on HR teams.
        </p>
        <div class="meta-row">
          <span>Industry: HR Tech / People Analytics</span>
          <span>Company: Moodbit (USA)</span>
          <span>Deployment: SaaS on Google Cloud</span>
        </div>
      </section>

      <!-- SECTION: GOOGLE CLOUD INTEGRATION -->
      <section class="section reveal delay-1">
        <div class="section-header">
          <h2 class="section-title">
            Deep integration with Google Cloud
            <span class="section-pill">GOOGLE CLOUD NATIVE</span>
          </h2>
          <div class="section-kicker">
            Secure, scalable and compliant by design.
          </div>
        </div>
        <p>
          Moodbit’s architecture is built natively on <strong>Google Cloud</strong> to deliver reliable performance and enterprise-grade security. <strong>Vertex AI</strong> orchestrates large language models and embeddings used by our AI agents and Cognitive Search layer, combining system prompts with customer-specific HR context stored in BigQuery. <strong>BigQuery</strong> serves as the analytical backbone, storing structured HR metrics and aggregations for sentiment, attrition risk and engagement indexes.
        </p>
        <p>
          The application backend is deployed on <strong>Cloud Run</strong>, providing containerized, auto-scaling services exposed behind <strong>Cloud Load Balancing</strong>. HR documents, contracts and attachments are securely stored in <strong>Cloud Storage</strong> and indexed for semantic retrieval. <strong>Cloud Logging</strong> and <strong>Cloud Monitoring</strong> provide end-to-end observability, while <strong>Cloud IAM</strong>, <strong>VPC Service Controls</strong> and customer-specific encryption policies enforce access control and data protection across regions.
        </p>

        <div class="section-grid">
          <div class="section-card">
            <h4>Vertex AI for HR agents</h4>
            Moodbit uses Vertex AI for LLM hosting, embeddings, and prompt orchestration, enabling HR chat agents that can answer policy questions, summarize documents and recommend next best actions while respecting company rules.
          </div>
          <div class="section-card">
            <h4>BigQuery &amp; analytics</h4>
            HR events, survey responses, and sentiment metrics are consolidated in BigQuery. This allows advanced cohort analysis, retention modeling and KPI tracking powered by SQL, Looker Studio and custom dashboards.
          </div>
          <div class="section-card">
            <h4>Cloud Run &amp; APIs</h4>
            Stateless microservices on Cloud Run expose secure APIs to web and chat clients, integrate with HRIS, payroll and collaboration tools, and scale automatically with usage peaks during performance cycles.
          </div>
        </div>
      </section>

      <!-- SECTION: USE CASES & VALUE -->
      <section class="section reveal delay-2">
        <div class="section-header">
          <h2 class="section-title">
            Key HR use cases &amp; business value
            <span class="section-pill">OUTCOMES</span>
          </h2>
          <div class="section-kicker">
            From HR operations to strategic workforce decisions.
          </div>
        </div>
        <p>
          With Moodbit on Google Cloud, organizations gain a unified, intelligent layer on top of existing HR systems. HR teams can automate repetitive inquiries (leave, benefits, payroll, policies), monitor sentiment in real time and simulate workforce scenarios using trusted data. Managers receive contextual alerts on engagement risk and team health, while employees enjoy a consumer-grade experience for HR support.
        </p>
        <div class="section-grid">
          <div class="section-card">
            <h4>Operational efficiency</h4>
            Reduce manual ticket handling and email back-and-forth by enabling AI agents to resolve the majority of HR questions instantly, freeing HR specialists to focus on strategic initiatives.
          </div>
          <div class="section-card">
            <h4>Employee experience</h4>
            Provide 24/7, personalized support in the tools employees already use, with answers grounded in verified HR data and policies, improving satisfaction and perceived transparency.
          </div>
          <div class="section-card">
            <h4>Data-driven decisions</h4>
            Use predictive indicators built in BigQuery and surfaced through Moodbit to inform hiring, retention, and wellbeing strategies, backed by auditable, centralized HR data.
          </div>
        </div>
      </section>

      <!-- SECTION: SECURITY & COMPLIANCE -->
      <section class="section reveal delay-3">
        <div class="section-header">
          <h2 class="section-title">
            Security, privacy &amp; compliance
            <span class="section-pill">TRUST</span>
          </h2>
          <div class="section-kicker">
            Protecting sensitive HR data is foundational.
          </div>
        </div>
        <p>
          HR data is inherently sensitive. Moodbit leverages Google Cloud’s security capabilities to enforce strong protections across data lifecycle. All data is encrypted in transit and at rest using Google-managed or customer-managed keys. Access is restricted via fine-grained IAM policies, with per-tenant isolation at the data and application layers. Data residency options and retention policies are aligned with customer and regulatory requirements.
        </p>
        <p>
          Logging and audit trails are centralized in Cloud Logging, enabling customers to review access, investigate anomalies and integrate with existing SIEM tools. Model prompts and outputs are scoped to customer data only; Moodbit does not use customer content to train public models. Together with Google Cloud’s compliance posture, this architecture helps organizations meet internal governance standards and external regulations related to employee data.
        </p>
      </section>
    </main>

    <footer class="reveal">
      Moodbit · AI HR Intelligence on Google Cloud ·
      <a href="https://mymoodbit.cc" target="_blank" rel="noopener">mymoodbit.cc</a> ·
      Contact: <a href="mailto:alfredo@mymoodbit.cc">alfredo@mymoodbit.cc</a>
    </footer>
  </div>

  <!-- === JS: cinematic background + scroll reveal === -->
  <script>
    // Animated network background
    (function () {
      const canvas = document.getElementById("bg-canvas");
      const ctx = canvas.getContext("2d");
      let width, height, points;

      function resize() {
        width = canvas.width = window.innerWidth * window.devicePixelRatio;
        height = canvas.height = window.innerHeight * window.devicePixelRatio;
      }

      function createPoints() {
        const count = Math.min(80, Math.floor(width / 40));
        points = [];
        for (let i = 0; i < count; i++) {
          points.push({
            x: Math.random() * width,
            y: Math.random() * height,
            vx: (Math.random() - 0.5) * 0.25 * window.devicePixelRatio,
            vy: (Math.random() - 0.5) * 0.25 * window.devicePixelRatio,
          });
        }
      }

      function draw() {
        ctx.clearRect(0, 0, width, height);
        ctx.fillStyle = "rgba(248, 250, 252, 0.9)";
        ctx.fillRect(0, 0, width, height);

        for (let i = 0; i < points.length; i++) {
          const p = points[i];
          p.x += p.vx;
          p.y += p.vy;

          if (p.x < 0 || p.x > width) p.vx *= -1;
          if (p.y < 0 || p.y > height) p.vy *= -1;

          ctx.beginPath();
          ctx.arc(p.x, p.y, 2.0 * window.devicePixelRatio, 0, Math.PI * 2);
          ctx.fillStyle = "rgba(129, 140, 248, 0.45)";
          ctx.fill();
        }

        const maxDist = 160 * window.devicePixelRatio;
        for (let i = 0; i < points.length; i++) {
          for (let j = i + 1; j < points.length; j++) {
            const p1 = points[i];
            const p2 = points[j];
            const dx = p1.x - p2.x;
            const dy = p1.y - p2.y;
            const dist = Math.sqrt(dx * dx + dy * dy);
            if (dist < maxDist) {
              const alpha = 1 - dist / maxDist;
              ctx.strokeStyle = "rgba(37, 99, 235," + (0.08 * alpha) + ")";
              ctx.lineWidth = 1 * window.devicePixelRatio;
              ctx.beginPath();
              ctx.moveTo(p1.x, p1.y);
              ctx.lineTo(p2.x, p2.y);
              ctx.stroke();
            }
          }
        }

        requestAnimationFrame(draw);
      }

      resize();
      createPoints();
      draw();
      window.addEventListener("resize", function () {
        resize();
        createPoints();
      });
    })();

    // Scroll reveal
    (function () {
      const elements = document.querySelectorAll(".reveal");
      const observer = new IntersectionObserver(
        (entries) => {
          entries.forEach((entry) => {
            if (entry.isIntersecting) {
              entry.target.classList.add("visible");
              observer.unobserve(entry.target);
            }
          });
        },
        { threshold: 0.12 }
      );

      elements.forEach((el) => observer.observe(el));
    })();
  </script>
</body>
</html>
