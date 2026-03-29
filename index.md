<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AI News Aggregator</title>
    <meta name="description" content="AI-powered news aggregator for the latest AI research, products, and breakthroughs.">
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800&display=swap" rel="stylesheet">
    <style>
        *, *::before, *::after { margin: 0; padding: 0; box-sizing: border-box; }

        :root {
            --bg-primary: #ffffff;
            --bg-secondary: #f8f9fb;
            --bg-card: #ffffff;
            --border-color: #e8eaed;
            --border-hover: #d0d4da;
            --text-primary: #1a1a2e;
            --text-secondary: #4a4a68;
            --text-muted: #8c8ca1;
            --accent: #4f46e5;
            --accent-light: #eef2ff;
            --youtube-color: #ff4444;
            --youtube-bg: #fff0f0;
            --openai-color: #10b981;
            --openai-bg: #ecfdf5;
            --anthropic-color: #f59e0b;
            --anthropic-bg: #fffbeb;
            --shadow-sm: 0 1px 3px rgba(0,0,0,0.04);
            --shadow-md: 0 4px 16px rgba(0,0,0,0.06);
            --shadow-lg: 0 8px 30px rgba(0,0,0,0.08);
            --radius: 14px;
        }

        body {
            font-family: 'Inter', -apple-system, BlinkMacSystemFont, sans-serif;
            background: var(--bg-secondary);
            color: var(--text-primary);
            min-height: 100vh;
            -webkit-font-smoothing: antialiased;
        }

        /* ─── Header ─── */
        .header {
            background: var(--bg-primary);
            border-bottom: 1px solid var(--border-color);
            padding: 16px 0;
            position: sticky;
            top: 0;
            z-index: 100;
        }

        .header-inner {
            max-width: 1240px;
            margin: 0 auto;
            padding: 0 24px;
            display: flex;
            align-items: center;
            justify-content: space-between;
            gap: 24px;
        }

        .logo {
            display: flex;
            align-items: center;
            gap: 10px;
            flex-shrink: 0;
        }

        .logo-icon {
            width: 36px;
            height: 36px;
            border-radius: 10px;
            background: var(--accent);
            display: flex;
            align-items: center;
            justify-content: center;
            color: white;
            font-size: 18px;
        }

        .logo h1 {
            font-size: 18px;
            font-weight: 700;
            color: var(--accent);
            letter-spacing: -0.3px;
        }

        .search-container {
            flex: 1;
            max-width: 420px;
            position: relative;
        }

        .search-container .search-icon {
            position: absolute;
            left: 14px;
            top: 50%;
            transform: translateY(-50%);
            color: var(--text-muted);
            font-size: 14px;
            pointer-events: none;
        }

        .search-container input {
            width: 100%;
            padding: 10px 16px 10px 40px;
            border-radius: 10px;
            border: 1px solid var(--border-color);
            background: var(--bg-secondary);
            color: var(--text-primary);
            font-family: 'Inter', sans-serif;
            font-size: 13.5px;
            outline: none;
            transition: all 0.2s ease;
        }

        .search-container input::placeholder { color: var(--text-muted); }
        .search-container input:focus {
            border-color: var(--accent);
            box-shadow: 0 0 0 3px rgba(79, 70, 229, 0.1);
            background: var(--bg-primary);
        }

        .header-actions {
            display: flex;
            align-items: center;
            gap: 10px;
            flex-shrink: 0;
        }

        .btn-pipeline {
            display: flex;
            align-items: center;
            gap: 7px;
            padding: 9px 18px;
            border-radius: 10px;
            border: none;
            background: var(--accent);
            color: white;
            font-family: 'Inter', sans-serif;
            font-size: 13px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.2s ease;
        }

        .btn-pipeline:hover {
            background: #4338ca;
            box-shadow: 0 4px 12px rgba(79, 70, 229, 0.3);
        }

        .btn-pipeline:disabled { opacity: 0.55; cursor: not-allowed; }

        .btn-pipeline .spinner {
            width: 13px;
            height: 13px;
            border: 2px solid rgba(255,255,255,0.3);
            border-top-color: white;
            border-radius: 50%;
            animation: spin 0.6s linear infinite;
            display: none;
        }
        .btn-pipeline.loading .spinner { display: block; }
        .btn-pipeline.loading .btn-icon { display: none; }

        @keyframes spin { to { transform: rotate(360deg); } }

        .pipeline-indicator {
            display: flex;
            align-items: center;
            gap: 6px;
            font-size: 11px;
            color: var(--text-muted);
            padding: 6px 12px;
            border-radius: 20px;
            background: var(--bg-secondary);
            border: 1px solid var(--border-color);
        }

        .status-dot {
            width: 6px;
            height: 6px;
            border-radius: 50%;
            background: var(--text-muted);
        }

        .status-dot.running {
            background: #10b981;
            box-shadow: 0 0 6px rgba(16,185,129,0.5);
            animation: pulse-dot 1.4s infinite ease-in-out;
        }

        @keyframes pulse-dot {
            0%, 100% { opacity: 1; transform: scale(1); }
            50% { opacity: 0.4; transform: scale(1.5); }
        }

        /* ─── Category Nav ─── */
        .category-nav {
            max-width: 1240px;
            margin: 0 auto;
            padding: 20px 24px 0;
            display: flex;
            align-items: center;
            gap: 8px;
            flex-wrap: wrap;
        }

        .cat-pill {
            padding: 8px 20px;
            border-radius: 24px;
            border: none;
            background: transparent;
            color: var(--text-secondary);
            font-family: 'Inter', sans-serif;
            font-size: 13px;
            font-weight: 500;
            cursor: pointer;
            transition: all 0.2s ease;
        }

        .cat-pill:hover {
            background: var(--bg-primary);
            color: var(--text-primary);
        }

        .cat-pill.active {
            background: var(--accent);
            color: white;
        }

        /* ─── Main Content ─── */
        .main-content {
            max-width: 1240px;
            margin: 0 auto;
            padding: 28px 24px 60px;
        }

        .section-header {
            margin-bottom: 24px;
        }

        .section-header h2 {
            font-size: 26px;
            font-weight: 800;
            letter-spacing: -0.5px;
            margin-bottom: 4px;
        }

        .section-header .subtitle {
            font-size: 14px;
            color: var(--text-muted);
        }

        /* ─── Card Grid ─── */
        .card-grid {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 24px;
        }

        .news-card {
            background: var(--bg-card);
            border-radius: var(--radius);
            border: 1px solid var(--border-color);
            overflow: hidden;
            transition: all 0.3s ease;
            text-decoration: none;
            color: inherit;
            display: flex;
            flex-direction: column;
            opacity: 0;
            transform: translateY(16px);
            animation: fadeUp 0.45s ease forwards;
        }

        .news-card:hover {
            transform: translateY(-4px);
            box-shadow: var(--shadow-lg);
            border-color: var(--border-hover);
        }

        @keyframes fadeUp {
            to { opacity: 1; transform: translateY(0); }
        }

        .card-image {
            width: 100%;
            height: 200px;
            position: relative;
            overflow: hidden;
        }

        .card-image-bg {
            width: 100%;
            height: 100%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 42px;
            position: relative;
        }

        .card-image-bg.youtube {
            background: linear-gradient(135deg, #1a1a2e 0%, #2d1b4e 40%, #4a1942 100%);
        }
        .card-image-bg.openai {
            background: linear-gradient(135deg, #0f172a 0%, #1e3a5f 40%, #0d4f4f 100%);
        }
        .card-image-bg.anthropic {
            background: linear-gradient(135deg, #1a1a2e 0%, #3d2b1f 40%, #4a3520 100%);
        }

        /* Decorative shapes in card image */
        .card-image-bg::before {
            content: '';
            position: absolute;
            width: 140px;
            height: 140px;
            border-radius: 50%;
            opacity: 0.1;
            top: -30px;
            right: -30px;
        }

        .card-image-bg.youtube::before { background: var(--youtube-color); }
        .card-image-bg.openai::before { background: var(--openai-color); }
        .card-image-bg.anthropic::before { background: var(--anthropic-color); }

        .card-image-bg::after {
            content: '';
            position: absolute;
            width: 200px;
            height: 200px;
            border-radius: 50%;
            opacity: 0.06;
            bottom: -60px;
            left: -40px;
        }

        .card-image-bg.youtube::after { background: #ff6b6b; }
        .card-image-bg.openai::after { background: #34d399; }
        .card-image-bg.anthropic::after { background: #fbbf24; }

        .card-image-icon {
            font-size: 48px;
            opacity: 0.2;
            color: white;
            z-index: 1;
        }

        .card-category {
            position: absolute;
            top: 14px;
            left: 14px;
            padding: 5px 14px;
            border-radius: 20px;
            font-size: 11px;
            font-weight: 600;
            text-transform: capitalize;
            color: white;
            z-index: 2;
        }

        .card-category.youtube { background: var(--youtube-color); }
        .card-category.openai { background: var(--openai-color); }
        .card-category.anthropic { background: var(--anthropic-color); }

        .card-body {
            padding: 20px 22px;
            flex: 1;
            display: flex;
            flex-direction: column;
        }

        .card-title {
            font-size: 16px;
            font-weight: 700;
            line-height: 1.4;
            margin-bottom: 10px;
            color: var(--text-primary);
            letter-spacing: -0.2px;
        }

        .card-summary {
            font-size: 13.5px;
            line-height: 1.65;
            color: var(--text-secondary);
            margin-bottom: 16px;
            flex: 1;
        }

        .card-meta {
            display: flex;
            align-items: center;
            gap: 6px;
            font-size: 12px;
            color: var(--text-muted);
            margin-bottom: 14px;
        }

        .card-meta .dot {
            width: 3px;
            height: 3px;
            border-radius: 50%;
            background: var(--text-muted);
        }

        .card-source {
            color: var(--accent);
            font-weight: 600;
        }

        .card-read-more {
            display: inline-flex;
            align-items: center;
            gap: 6px;
            font-size: 13px;
            font-weight: 600;
            color: var(--text-primary);
            transition: gap 0.2s ease;
        }

        .news-card:hover .card-read-more { gap: 10px; color: var(--accent); }

        .card-read-more svg {
            width: 14px;
            height: 14px;
            stroke: currentColor;
            fill: none;
            stroke-width: 2;
        }

        /* ─── Empty State ─── */
        .empty-state {
            grid-column: 1 / -1;
            text-align: center;
            padding: 80px 20px;
        }

        .empty-state .empty-icon {
            font-size: 52px;
            margin-bottom: 18px;
            opacity: 0.25;
        }

        .empty-state h3 {
            font-size: 20px;
            font-weight: 700;
            color: var(--text-primary);
            margin-bottom: 8px;
        }

        .empty-state p {
            font-size: 14px;
            color: var(--text-muted);
        }

        /* ─── Toast ─── */
        .toast {
            position: fixed;
            bottom: 24px;
            right: 24px;
            padding: 14px 22px;
            border-radius: 12px;
            font-size: 13px;
            font-weight: 500;
            z-index: 1000;
            transform: translateY(80px);
            opacity: 0;
            transition: all 0.4s cubic-bezier(0.16, 1, 0.3, 1);
            background: var(--text-primary);
            color: white;
            box-shadow: var(--shadow-lg);
        }

        .toast.error { background: var(--youtube-color); }
        .toast.show { transform: translateY(0); opacity: 1; }

        /* ─── Loading skeleton ─── */
        .skeleton-card {
            background: var(--bg-card);
            border-radius: var(--radius);
            border: 1px solid var(--border-color);
            overflow: hidden;
        }

        .skeleton-image {
            height: 200px;
            background: linear-gradient(90deg, #f0f0f5 25%, #e8e8f0 50%, #f0f0f5 75%);
            background-size: 200% 100%;
            animation: shimmer 1.5s infinite;
        }

        .skeleton-body { padding: 20px 22px; }

        .skeleton-line {
            height: 14px;
            border-radius: 6px;
            background: linear-gradient(90deg, #f0f0f5 25%, #e8e8f0 50%, #f0f0f5 75%);
            background-size: 200% 100%;
            animation: shimmer 1.5s infinite;
            margin-bottom: 10px;
        }

        .skeleton-line:last-child { width: 60%; }

        @keyframes shimmer {
            0% { background-position: -200% 0; }
            100% { background-position: 200% 0; }
        }

        /* ─── Responsive ─── */
        @media (max-width: 1024px) {
            .card-grid { grid-template-columns: repeat(2, 1fr); }
        }

        @media (max-width: 768px) {
            .header-inner { flex-wrap: wrap; }
            .search-container { order: 3; max-width: 100%; flex-basis: 100%; }
            .card-grid { grid-template-columns: 1fr; }
            .section-header h2 { font-size: 22px; }
        }

        @media (max-width: 480px) {
            .header-actions { gap: 6px; }
            .btn-pipeline { padding: 8px 14px; font-size: 12px; }
            .card-image { height: 160px; }
        }
    </style>
</head>
<body>
    <!-- Header -->
    <header class="header">
        <div class="header-inner">
            <div class="logo">
                <div class="logo-icon">⚡</div>
                <h1>AI News Aggregator</h1>
            </div>
            <div class="search-container">
                <span class="search-icon">🔍</span>
                <input type="text" id="searchInput" placeholder="Search news..." oninput="handleSearch()">
            </div>
            <div class="header-actions">
                <div class="pipeline-indicator">
                    <div class="status-dot" id="statusDot"></div>
                    <span id="statusText">Idle</span>
                </div>
                <button class="btn-pipeline" id="btnRun" onclick="runPipeline()">
                    <span class="spinner"></span>
                    <span class="btn-icon">▶</span>
                    Run Pipeline
                </button>
            </div>
        </div>
    </header>

    <!-- Category Nav -->
    <nav class="category-nav" id="categoryNav">
        <button class="cat-pill active" data-filter="all" onclick="setFilter('all', this)">All</button>
        <button class="cat-pill" data-filter="youtube" onclick="setFilter('youtube', this)">YouTube</button>
        <button class="cat-pill" data-filter="openai" onclick="setFilter('openai', this)">OpenAI</button>
        <button class="cat-pill" data-filter="anthropic" onclick="setFilter('anthropic', this)">Anthropic</button>
    </nav>

    <!-- Main Content -->
    <main class="main-content">
        <div class="section-header">
            <h2>Latest News</h2>
            <p class="subtitle" id="articleCount">Loading articles...</p>
        </div>

        <div class="card-grid" id="cardGrid">
            <!-- Skeleton loaders -->
            <div class="skeleton-card"><div class="skeleton-image"></div><div class="skeleton-body"><div class="skeleton-line"></div><div class="skeleton-line"></div><div class="skeleton-line"></div></div></div>
            <div class="skeleton-card"><div class="skeleton-image"></div><div class="skeleton-body"><div class="skeleton-line"></div><div class="skeleton-line"></div><div class="skeleton-line"></div></div></div>
            <div class="skeleton-card"><div class="skeleton-image"></div><div class="skeleton-body"><div class="skeleton-line"></div><div class="skeleton-line"></div><div class="skeleton-line"></div></div></div>
        </div>
    </main>

    <!-- Toast -->
    <div class="toast" id="toast"></div>

    <script>
        let allDigests = [];
        let currentFilter = 'all';
        let searchQuery = '';
        let pipelineCheckInterval = null;

        // ─── Init ───
        document.addEventListener('DOMContentLoaded', () => {
            loadDigests();
            checkPipelineStatus();
        });

        // ─── Demo Data ───
        const DEMO_DIGESTS = [
            {
                id: "demo-1", article_type: "openai", title: "GPT-5 Achieves Breakthrough in Mathematical Reasoning",
                summary: "OpenAI's latest model demonstrates unprecedented capability in solving complex mathematical proofs, outperforming previous benchmarks by 40%. Researchers highlight its ability to chain multi-step logical deductions across abstract algebra and topology problems.",
                url: "#", created_at: "2026-03-24T10:00:00Z"
            },
            {
                id: "demo-2", article_type: "youtube", title: "Building a Full-Stack AI Agent From Scratch — Live Coding",
                summary: "Matthew Berman walks through building a production-ready AI agent with tool use, memory, and planning capabilities using LangGraph and GPT-4o. Covers architecture patterns, error handling, and real-world deployment tips.",
                url: "#", created_at: "2026-03-24T08:30:00Z"
            },
            {
                id: "demo-3", article_type: "anthropic", title: "Claude 4 Introduces Native Multi-Modal Reasoning",
                summary: "Anthropic announces Claude 4 with groundbreaking vision-language integration. The model can analyze complex diagrams, charts, and handwritten notes while maintaining context across 200K token conversations.",
                url: "#", created_at: "2026-03-23T18:00:00Z"
            },
            {
                id: "demo-4", article_type: "openai", title: "New Research: Scaling Laws for Retrieval-Augmented Generation",
                summary: "A new paper from OpenAI explores how RAG system performance scales with chunk size, embedding dimensions, and retrieval depth. Findings suggest optimal configurations vary dramatically across domain types.",
                url: "#", created_at: "2026-03-23T14:00:00Z"
            },
            {
                id: "demo-5", article_type: "youtube", title: "The Future of AI Agents — Stanford HAI Panel Discussion",
                summary: "Leading researchers from Stanford discuss the trajectory of autonomous AI agents, covering safety concerns, economic implications, and the technical challenges of building reliable multi-step reasoning systems.",
                url: "#", created_at: "2026-03-23T11:00:00Z"
            },
            {
                id: "demo-6", article_type: "anthropic", title: "Constitutional AI 2.0: Self-Improving Safety Alignment",
                summary: "Anthropic publishes new findings on their constitutional AI approach, demonstrating how models can iteratively refine their own safety guidelines while maintaining helpfulness across diverse use cases.",
                url: "#", created_at: "2026-03-22T20:00:00Z"
            },
            {
                id: "demo-7", article_type: "openai", title: "Codex Next-Gen: AI-Powered Software Engineering at Scale",
                summary: "OpenAI reveals the next evolution of Codex, capable of understanding entire codebases, generating architecture proposals, and autonomously implementing feature requests across multiple files and languages.",
                url: "#", created_at: "2026-03-22T16:00:00Z"
            },
            {
                id: "demo-8", article_type: "youtube", title: "RAG vs Fine-Tuning: When to Use What — Complete Guide",
                summary: "Dave Ebbelaar provides a comprehensive comparison of RAG and fine-tuning approaches, with benchmarks across 8 different use cases including customer support, code generation, and medical Q&A systems.",
                url: "#", created_at: "2026-03-22T09:00:00Z"
            },
            {
                id: "demo-9", article_type: "anthropic", title: "Sparse Attention Mechanisms Cut Inference Costs by 60%",
                summary: "New research from Anthropic demonstrates how sparse attention patterns can dramatically reduce computing requirements without sacrificing model quality, making large-scale deployments more economically viable.",
                url: "#", created_at: "2026-03-21T15:00:00Z"
            },
            {
                id: "demo-10", article_type: "openai", title: "DALL·E 4 Sets New Standard for Photorealistic Generation",
                summary: "The latest image generation model produces near-indistinguishable photorealistic outputs with precise text rendering, spatial consistency, and unprecedented control over lighting and composition details.",
                url: "#", created_at: "2026-03-21T12:00:00Z"
            },
            {
                id: "demo-11", article_type: "youtube", title: "Deploy Your Own LLM on AWS — Step by Step Tutorial",
                summary: "A practical guide to deploying open-source LLMs on AWS using SageMaker endpoints, covering model quantization, auto-scaling, cost optimization, and setting up a production-ready inference API.",
                url: "#", created_at: "2026-03-21T07:00:00Z"
            },
            {
                id: "demo-12", article_type: "anthropic", title: "AI Safety Benchmark Suite: Evaluating Model Robustness",
                summary: "Anthropic releases an open-source benchmark suite for evaluating AI model safety across 15 categories including prompt injection, jailbreaking resistance, truthfulness, and harmful content refusal patterns.",
                url: "#", created_at: "2026-03-20T19:00:00Z"
            }
        ];

        // ─── API Calls ───
        async function loadDigests() {
            try {
                const res = await fetch('/api/digests?hours=8760');
                const data = await res.json();
                // Merge real data with demo data, real data first
                allDigests = [...data.digests, ...DEMO_DIGESTS];
                renderCards();
            } catch (e) {
                console.error('Failed to load digests:', e);
                // Fall back to demo data
                allDigests = [...DEMO_DIGESTS];
                renderCards();
            }
        }

        async function runPipeline() {
            const btn = document.getElementById('btnRun');
            if (btn.disabled) return;

            btn.classList.add('loading');
            btn.disabled = true;

            try {
                const res = await fetch('/api/pipeline/run', { method: 'POST' });
                const data = await res.json();

                if (res.status === 409) {
                    showToast('Pipeline is already running', 'error');
                    btn.classList.remove('loading');
                    btn.disabled = false;
                    return;
                }

                showToast('Pipeline started! Fetching latest news...');
                document.getElementById('statusDot').classList.add('running');
                document.getElementById('statusText').textContent = 'Running...';

                pipelineCheckInterval = setInterval(async () => {
                    const status = await (await fetch('/api/pipeline/status')).json();
                    if (!status.running) {
                        clearInterval(pipelineCheckInterval);
                        pipelineCheckInterval = null;
                        btn.classList.remove('loading');
                        btn.disabled = false;
                        document.getElementById('statusDot').classList.remove('running');
                        document.getElementById('statusText').textContent = 'Idle';

                        if (status.last_result?.success) {
                            showToast('Pipeline completed! ✓');
                        } else {
                            showToast('Pipeline finished with errors', 'error');
                        }
                        loadDigests();
                    }
                }, 3000);
            } catch (e) {
                showToast('Failed to start pipeline', 'error');
                btn.classList.remove('loading');
                btn.disabled = false;
            }
        }

        async function checkPipelineStatus() {
            try {
                const res = await fetch('/api/pipeline/status');
                const data = await res.json();
                if (data.running) {
                    document.getElementById('statusDot').classList.add('running');
                    document.getElementById('statusText').textContent = 'Running...';
                    document.getElementById('btnRun').classList.add('loading');
                    document.getElementById('btnRun').disabled = true;
                } else if (data.last_run) {
                    const d = new Date(data.last_run);
                    document.getElementById('statusText').textContent = `Last: ${d.toLocaleTimeString('en-US', { hour: '2-digit', minute: '2-digit' })}`;
                }
            } catch (e) {}
        }

        // ─── Rendering ───
        function renderCards() {
            const grid = document.getElementById('cardGrid');
            let filtered = [...allDigests];

            if (currentFilter !== 'all') {
                filtered = filtered.filter(d => d.article_type === currentFilter);
            }

            if (searchQuery) {
                const q = searchQuery.toLowerCase();
                filtered = filtered.filter(d =>
                    d.title.toLowerCase().includes(q) ||
                    d.summary.toLowerCase().includes(q)
                );
            }

            document.getElementById('articleCount').textContent = `${filtered.length} article${filtered.length !== 1 ? 's' : ''} found`;

            if (filtered.length === 0) {
                grid.innerHTML = renderEmptyState(
                    searchQuery ? 'No articles match your search' : 'No articles yet — run the pipeline!'
                );
                return;
            }

            grid.innerHTML = filtered.map((d, i) => `
                <a href="${escapeHtml(d.url)}" target="_blank" rel="noopener" class="news-card" style="animation-delay: ${i * 0.06}s">
                    <div class="card-image">
                        <div class="card-image-bg ${d.article_type}">
                            <span class="card-image-icon">${sourceIconLarge(d.article_type)}</span>
                        </div>
                        <span class="card-category ${d.article_type}">${capitalise(d.article_type)}</span>
                    </div>
                    <div class="card-body">
                        <div class="card-title">${escapeHtml(d.title)}</div>
                        <div class="card-summary">${escapeHtml(truncate(d.summary, 160))}</div>
                        <div class="card-meta">
                            <span>📅 ${formatDate(d.created_at)}</span>
                            <span class="dot"></span>
                            <span class="card-source">${capitalise(d.article_type)}</span>
                        </div>
                        <span class="card-read-more">
                            Read More
                            <svg viewBox="0 0 24 24"><path d="M7 17L17 7M17 7H7M17 7V17" stroke-linecap="round" stroke-linejoin="round"/></svg>
                        </span>
                    </div>
                </a>
            `).join('');
        }

        function renderEmptyState(message) {
            return `
                <div class="empty-state">
                    <div class="empty-icon">📰</div>
                    <h3>${message}</h3>
                    <p>Click "Run Pipeline" to fetch the latest AI news from all sources.</p>
                </div>
            `;
        }

        // ─── UI Interactions ───
        function setFilter(filter, btn) {
            currentFilter = filter;
            document.querySelectorAll('.cat-pill').forEach(b => b.classList.remove('active'));
            btn.classList.add('active');
            renderCards();
        }

        function handleSearch() {
            searchQuery = document.getElementById('searchInput').value;
            renderCards();
        }

        // ─── Utilities ───
        function formatDate(dateStr) {
            if (!dateStr) return '';
            const d = new Date(dateStr);
            return d.toLocaleDateString('en-GB', { day: '2-digit', month: '2-digit', year: 'numeric' });
        }

        function truncate(str, max) {
            if (!str) return '';
            return str.length > max ? str.slice(0, max) + '…' : str;
        }

        function capitalise(str) {
            if (!str) return '';
            return str.charAt(0).toUpperCase() + str.slice(1);
        }

        function escapeHtml(str) {
            if (!str) return '';
            const div = document.createElement('div');
            div.textContent = str;
            return div.innerHTML;
        }

        function sourceIconLarge(type) {
            const icons = { youtube: '▶', openai: '◉', anthropic: '◈' };
            return icons[type] || '●';
        }

        function showToast(message, type = 'success') {
            const toast = document.getElementById('toast');
            toast.textContent = message;
            toast.className = `toast ${type}`;
            requestAnimationFrame(() => toast.classList.add('show'));
            setTimeout(() => toast.classList.remove('show'), 4000);
        }
    </script>
</body>
</html>
