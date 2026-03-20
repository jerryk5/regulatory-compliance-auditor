<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>SPARK — AI Content Generator</title>
<link rel="preconnect" href="https://fonts.googleapis.com" />
<link href="https://fonts.googleapis.com/css2?family=Fraunces:ital,opsz,wght@0,9..144,300;0,9..144,600;1,9..144,300&family=JetBrains+Mono:wght@400;500&family=DM+Sans:wght@400;500&display=swap" rel="stylesheet" />
<style>
  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

  :root {
    --bg: #0D0C0A;
    --surface: #171612;
    --surface2: #1F1E19;
    --border: rgba(245,240,232,0.1);
    --border-hover: rgba(245,240,232,0.22);
    --cream: #F5F0E8;
    --cream-muted: rgba(245,240,232,0.5);
    --cream-faint: rgba(245,240,232,0.12);
    --amber: #E8A020;
    --amber-dim: rgba(232,160,32,0.15);
    --amber-glow: rgba(232,160,32,0.08);
    --red: #E05A4A;
    --green: #4CAF82;
    --font-display: 'Fraunces', serif;
    --font-mono: 'JetBrains Mono', monospace;
    --font-body: 'DM Sans', sans-serif;
  }

  html, body {
    min-height: 100vh;
    background: var(--bg);
    color: var(--cream);
    font-family: var(--font-body);
    font-size: 16px;
    line-height: 1.6;
    -webkit-font-smoothing: antialiased;
  }

  .app {
    min-height: 100vh;
    display: flex;
    flex-direction: column;
  }

  header {
    border-bottom: 1px solid var(--border);
    padding: 1.25rem 2rem;
    display: flex;
    align-items: center;
    justify-content: space-between;
    position: sticky;
    top: 0;
    background: var(--bg);
    z-index: 10;
  }

  .logo {
    font-family: var(--font-display);
    font-size: 1.5rem;
    font-weight: 600;
    letter-spacing: -0.02em;
    color: var(--cream);
    display: flex;
    align-items: center;
    gap: 0.5rem;
  }

  .logo-dot {
    width: 8px;
    height: 8px;
    background: var(--amber);
    border-radius: 50%;
    animation: pulse 2s ease-in-out infinite;
  }

  @keyframes pulse {
    0%, 100% { opacity: 1; transform: scale(1); }
    50% { opacity: 0.5; transform: scale(0.7); }
  }

  .tagline {
    font-size: 0.75rem;
    color: var(--cream-muted);
    font-family: var(--font-mono);
    letter-spacing: 0.05em;
  }

  main {
    flex: 1;
    max-width: 760px;
    margin: 0 auto;
    width: 100%;
    padding: 3rem 2rem;
    display: flex;
    flex-direction: column;
    gap: 2.5rem;
  }

  .hero-text {
    text-align: center;
  }

  .hero-text h1 {
    font-family: var(--font-display);
    font-size: clamp(2.2rem, 5vw, 3.5rem);
    font-weight: 300;
    letter-spacing: -0.03em;
    line-height: 1.1;
    color: var(--cream);
    margin-bottom: 0.75rem;
  }

  .hero-text h1 em {
    font-style: italic;
    color: var(--amber);
  }

  .hero-text p {
    color: var(--cream-muted);
    font-size: 0.95rem;
  }

  .section-label {
    font-family: var(--font-mono);
    font-size: 0.65rem;
    text-transform: uppercase;
    letter-spacing: 0.12em;
    color: var(--cream-muted);
    margin-bottom: 0.75rem;
  }

  .modes-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(140px, 1fr));
    gap: 0.5rem;
  }

  .mode-btn {
    border: 1px solid var(--border);
    background: var(--surface);
    color: var(--cream-muted);
    padding: 0.75rem 1rem;
    border-radius: 10px;
    cursor: pointer;
    font-family: var(--font-body);
    font-size: 0.85rem;
    font-weight: 500;
    text-align: left;
    transition: all 0.18s ease;
    display: flex;
    flex-direction: column;
    gap: 0.2rem;
  }

  .mode-btn:hover {
    border-color: var(--border-hover);
    color: var(--cream);
    background: var(--surface2);
  }

  .mode-btn.active {
    border-color: var(--amber);
    background: var(--amber-dim);
    color: var(--cream);
  }

  .mode-btn .mode-icon {
    font-size: 1.1rem;
    line-height: 1;
    margin-bottom: 0.1rem;
  }

  .mode-btn .mode-name {
    font-size: 0.82rem;
    font-weight: 500;
  }

  .mode-btn .mode-desc {
    font-size: 0.7rem;
    color: var(--cream-muted);
    line-height: 1.3;
  }

  .mode-btn.active .mode-desc {
    color: rgba(245,240,232,0.6);
  }

  .input-area {
    display: flex;
    flex-direction: column;
    gap: 0.5rem;
  }

  textarea {
    width: 100%;
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 12px;
    color: var(--cream);
    font-family: var(--font-body);
    font-size: 0.95rem;
    line-height: 1.6;
    padding: 1rem 1.1rem;
    resize: none;
    outline: none;
    transition: border-color 0.18s ease;
    min-height: 100px;
  }

  textarea::placeholder { color: var(--cream-muted); }
  textarea:focus { border-color: var(--border-hover); }

  .generate-btn {
    width: 100%;
    padding: 0.9rem;
    border-radius: 10px;
    border: none;
    background: var(--amber);
    color: #0D0C0A;
    font-family: var(--font-body);
    font-size: 0.95rem;
    font-weight: 500;
    cursor: pointer;
    transition: all 0.18s ease;
    letter-spacing: 0.01em;
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 0.5rem;
  }

  .generate-btn:hover:not(:disabled) { background: #f0aa28; transform: translateY(-1px); }
  .generate-btn:active:not(:disabled) { transform: translateY(0); }
  .generate-btn:disabled { opacity: 0.5; cursor: not-allowed; }

  .spinner {
    width: 16px;
    height: 16px;
    border: 2px solid rgba(13,12,10,0.3);
    border-top-color: #0D0C0A;
    border-radius: 50%;
    animation: spin 0.7s linear infinite;
  }

  @keyframes spin { to { transform: rotate(360deg); } }

  .output-card {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 14px;
    overflow: hidden;
    animation: slideUp 0.35s ease;
  }

  @keyframes slideUp {
    from { opacity: 0; transform: translateY(12px); }
    to   { opacity: 1; transform: translateY(0); }
  }

  .output-header {
    padding: 0.75rem 1.1rem;
    border-bottom: 1px solid var(--border);
    display: flex;
    align-items: center;
    justify-content: space-between;
    background: var(--surface2);
  }

  .output-badge {
    font-family: var(--font-mono);
    font-size: 0.65rem;
    letter-spacing: 0.1em;
    text-transform: uppercase;
    color: var(--amber);
    background: var(--amber-glow);
    padding: 0.25rem 0.6rem;
    border-radius: 6px;
    border: 1px solid rgba(232,160,32,0.2);
  }

  .copy-btn {
    background: transparent;
    border: 1px solid var(--border);
    color: var(--cream-muted);
    font-family: var(--font-body);
    font-size: 0.75rem;
    padding: 0.3rem 0.7rem;
    border-radius: 6px;
    cursor: pointer;
    transition: all 0.15s ease;
    display: flex;
    align-items: center;
    gap: 0.35rem;
  }

  .copy-btn:hover { border-color: var(--border-hover); color: var(--cream); }
  .copy-btn.copied { border-color: var(--green); color: var(--green); }

  .output-body {
    padding: 1.5rem 1.25rem;
    font-family: var(--font-mono);
    font-size: 0.875rem;
    line-height: 1.8;
    color: var(--cream);
    white-space: pre-wrap;
    word-break: break-word;
  }

  .output-body.streaming {
    position: relative;
  }

  .cursor-blink {
    display: inline-block;
    width: 2px;
    height: 1em;
    background: var(--amber);
    vertical-align: text-bottom;
    animation: blink 1s step-end infinite;
  }

  @keyframes blink {
    0%, 100% { opacity: 1; }
    50% { opacity: 0; }
  }

  .error-box {
    background: rgba(224,90,74,0.1);
    border: 1px solid rgba(224,90,74,0.3);
    border-radius: 10px;
    padding: 1rem 1.1rem;
    color: #f08070;
    font-size: 0.875rem;
    display: flex;
    align-items: flex-start;
    gap: 0.6rem;
  }

  .remix-row {
    display: flex;
    gap: 0.5rem;
    padding: 0.75rem 1.1rem;
    border-top: 1px solid var(--border);
    background: var(--surface2);
  }

  .remix-btn {
    flex: 1;
    padding: 0.5rem;
    border-radius: 8px;
    border: 1px solid var(--border);
    background: transparent;
    color: var(--cream-muted);
    font-family: var(--font-body);
    font-size: 0.78rem;
    cursor: pointer;
    transition: all 0.15s ease;
    text-align: center;
  }

  .remix-btn:hover {
    border-color: var(--border-hover);
    color: var(--cream);
    background: var(--cream-faint);
  }

  .char-count {
    text-align: right;
    font-family: var(--font-mono);
    font-size: 0.65rem;
    color: var(--cream-muted);
    margin-top: -0.25rem;
  }

  footer {
    padding: 1.5rem 2rem;
    text-align: center;
    font-size: 0.72rem;
    color: var(--cream-muted);
    border-top: 1px solid var(--border);
    font-family: var(--font-mono);
    letter-spacing: 0.03em;
  }

  @media (max-width: 500px) {
    main { padding: 2rem 1rem; gap: 2rem; }
    header { padding: 1rem; }
    .modes-grid { grid-template-columns: 1fr 1fr; }
  }
</style>
</head>
<body>
<div class="app">
  <header>
    <div class="logo">
      <div class="logo-dot"></div>
      SPARK
    </div>
    <div class="tagline">AI Content Generator</div>
  </header>

  <main>
    <div class="hero-text">
      <h1>What should we <em>create</em> today?</h1>
      <p>Pick a format, describe your idea, and let the AI do the rest.</p>
    </div>

    <div>
      <div class="section-label">01 — Choose a format</div>
      <div class="modes-grid" id="modesGrid"></div>
    </div>

    <div class="input-area">
      <div class="section-label">02 — Describe your topic or idea</div>
      <textarea id="topicInput" rows="4" placeholder="e.g. 'A sustainable coffee brand targeting Gen Z in East Africa'"></textarea>
      <div class="char-count" id="charCount">0 / 300</div>
      <button class="generate-btn" id="generateBtn" onclick="generate()">
        <span id="btnIcon">✦</span>
        <span id="btnLabel">Generate</span>
      </button>
    </div>

    <div id="outputArea"></div>
  </main>

  <footer>powered by claude · made with spark</footer>
</div>

<script>
const MODES = [
  {
    id: 'social',
    icon: '📣',
    name: 'Social caption',
    desc: 'Hook + copy for any platform',
    system: 'You are a world-class social media copywriter. Write punchy, engaging social media captions that stop the scroll. Include a hook, 2-3 lines of copy, a call-to-action, and 5 relevant hashtags. Be creative, platform-agnostic, and match the brand voice implied by the topic.',
    prompt: (t) => `Write a compelling social media caption for: ${t}`
  },
  {
    id: 'startup',
    icon: '🚀',
    name: 'Startup idea',
    desc: 'Name, tagline & pitch',
    system: 'You are a visionary startup ideator and pitch writer. Generate innovative, viable startup ideas with a catchy name, one-line tagline, problem statement, solution, target market, and a brief elevator pitch. Be bold and original.',
    prompt: (t) => `Generate a startup idea in this space: ${t}`
  },
  {
    id: 'story',
    icon: '✍️',
    name: 'Story opener',
    desc: 'Gripping first paragraph',
    system: 'You are a literary author known for unforgettable opening lines and atmospheric prose. Write a compelling story opening that drops readers into the scene with vivid sensory detail and immediate tension or intrigue. Make it 3-5 sentences, absolutely riveting.',
    prompt: (t) => `Write a gripping story opening about: ${t}`
  },
  {
    id: 'adcopy',
    icon: '💡',
    name: 'Ad copy',
    desc: 'Headline + body + CTA',
    system: 'You are a legendary advertising copywriter. Write conversion-focused ad copy with a powerful headline, compelling body copy (2-3 sentences), and a clear call to action. Use proven persuasion principles: benefit-first, emotional resonance, specificity.',
    prompt: (t) => `Write high-converting ad copy for: ${t}`
  },
  {
    id: 'email',
    icon: '📧',
    name: 'Email subject',
    desc: '5 subject line variations',
    system: 'You are an expert email marketer with deep knowledge of open rate optimization. Generate 5 email subject line variations for the given topic: one curiosity-driven, one benefit-driven, one urgency-driven, one personal/conversational, and one bold/provocative. Include the open rate style in brackets.',
    prompt: (t) => `Generate 5 email subject line variations for: ${t}`
  },
  {
    id: 'tagline',
    icon: '🎯',
    name: 'Brand tagline',
    desc: 'Memorable brand phrases',
    system: 'You are a branding genius who has crafted taglines for iconic global brands. Generate 5 powerful, memorable brand taglines. Each should be short (under 7 words), distinctive, and capture the essence of the brand. Explain the angle behind each one in one sentence.',
    prompt: (t) => `Create 5 brand taglines for: ${t}`
  }
];

let selectedMode = MODES[0];
let isLoading = false;
let abortController = null;

function renderModes() {
  const grid = document.getElementById('modesGrid');
  grid.innerHTML = MODES.map(m => `
    <button class="mode-btn ${m.id === selectedMode.id ? 'active' : ''}" onclick="selectMode('${m.id}')">
      <span class="mode-icon">${m.icon}</span>
      <span class="mode-name">${m.name}</span>
      <span class="mode-desc">${m.desc}</span>
    </button>
  `).join('');
}

function selectMode(id) {
  selectedMode = MODES.find(m => m.id === id);
  renderModes();
  document.getElementById('topicInput').focus();
}

const textarea = document.getElementById('topicInput');
textarea.addEventListener('input', () => {
  const len = Math.min(textarea.value.length, 300);
  document.getElementById('charCount').textContent = `${len} / 300`;
  if (textarea.value.length > 300) textarea.value = textarea.value.slice(0, 300);
});

textarea.addEventListener('keydown', (e) => {
  if (e.key === 'Enter' && e.metaKey) generate();
});

function setLoading(val) {
  isLoading = val;
  const btn = document.getElementById('generateBtn');
  const icon = document.getElementById('btnIcon');
  const label = document.getElementById('btnLabel');
  btn.disabled = val;
  if (val) {
    icon.innerHTML = '<div class="spinner"></div>';
    label.textContent = 'Generating…';
  } else {
    icon.textContent = '✦';
    label.textContent = 'Generate';
  }
}

function showError(msg) {
  document.getElementById('outputArea').innerHTML = `
    <div class="error-box">
      <span>⚠</span>
      <span>${msg}</span>
    </div>
  `;
}

async function generate() {
  const topic = document.getElementById('topicInput').value.trim();
  if (!topic) {
    document.getElementById('topicInput').focus();
    document.getElementById('topicInput').style.borderColor = 'rgba(224,90,74,0.6)';
    setTimeout(() => document.getElementById('topicInput').style.borderColor = '', 1200);
    return;
  }

  if (abortController) abortController.abort();
  abortController = new AbortController();
  setLoading(true);

  const outputArea = document.getElementById('outputArea');
  outputArea.innerHTML = `
    <div class="output-card" id="outputCard">
      <div class="output-header">
        <span class="output-badge">${selectedMode.icon} ${selectedMode.name}</span>
        <button class="copy-btn" id="copyBtn" onclick="copyResult()">Copy</button>
      </div>
      <div class="output-body streaming" id="outputBody"><span class="cursor-blink"></span></div>
    </div>
  `;

  try {
    const response = await fetch('https://api.anthropic.com/v1/messages', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      signal: abortController.signal,
      body: JSON.stringify({
        model: 'claude-sonnet-4-20250514',
        max_tokens: 1000,
        system: selectedMode.system,
        messages: [{ role: 'user', content: selectedMode.prompt(topic) }],
        stream: true
      })
    });

    if (!response.ok) {
      const err = await response.json();
      throw new Error(err.error?.message || 'API error');
    }

    const reader = response.body.getReader();
    const decoder = new TextDecoder();
    let fullText = '';
    const bodyEl = document.getElementById('outputBody');

    while (true) {
      const { done, value } = await reader.read();
      if (done) break;
      const chunk = decoder.decode(value, { stream: true });
      const lines = chunk.split('\n');
      for (const line of lines) {
        if (line.startsWith('data: ')) {
          const data = line.slice(6).trim();
          if (data === '[DONE]') continue;
          try {
            const parsed = JSON.parse(data);
            const delta = parsed.delta?.text || '';
            fullText += delta;
            if (bodyEl) bodyEl.innerHTML = escapeHtml(fullText) + '<span class="cursor-blink"></span>';
          } catch {}
        }
      }
    }

    if (document.getElementById('outputBody')) {
      document.getElementById('outputBody').innerHTML = escapeHtml(fullText);
      document.getElementById('outputBody').classList.remove('streaming');
    }

    const remixRow = document.createElement('div');
    remixRow.className = 'remix-row';
    remixRow.innerHTML = `
      <button class="remix-btn" onclick="remixWith('Make it shorter and punchier')">Shorter ↗</button>
      <button class="remix-btn" onclick="remixWith('Make it more formal and professional')">More formal ↗</button>
      <button class="remix-btn" onclick="remixWith('Make it more playful and fun')">More playful ↗</button>
      <button class="remix-btn" onclick="generate()">Regenerate ↻</button>
    `;
    document.getElementById('outputCard').appendChild(remixRow);

  } catch (err) {
    if (err.name !== 'AbortError') showError(err.message || 'Something went wrong. Please try again.');
  } finally {
    setLoading(false);
  }
}

function remixWith(instruction) {
  const currentOutput = document.getElementById('outputBody')?.textContent || '';
  const topic = document.getElementById('topicInput').value.trim();
  const input = document.getElementById('topicInput');
  input.value = `${topic}\n\n[Remix instruction: ${instruction}]\n\nOriginal output:\n${currentOutput.slice(0, 400)}`;
  generate();
  input.value = topic;
}

async function copyResult() {
  const text = document.getElementById('outputBody')?.textContent || '';
  await navigator.clipboard.writeText(text);
  const btn = document.getElementById('copyBtn');
  btn.textContent = '✓ Copied';
  btn.classList.add('copied');
  setTimeout(() => {
    btn.textContent = 'Copy';
    btn.classList.remove('copied');
  }, 2000);
}

function escapeHtml(str) {
  return str.replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/>/g,'&gt;');
}

renderModes();
</script>
</body>
</html>
