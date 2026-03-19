# OpenAI generation API

OpenAI generative services, including text, images, video, and other generation.

![Platform](https://img.shields.io/badge/platform-Ace%20Data%20Cloud-0f766e?style=flat-square) ![API](https://img.shields.io/badge/type-AI%20API-2563eb?style=flat-square) ![Docs](https://img.shields.io/badge/docs-online-16a34a?style=flat-square)

![OpenAI generation](https://cdn.acedata.cloud/20240225-225340.png/thumb_600x300)

API home page: [Ace Data Cloud - OpenAI generation](https://platform.acedata.cloud/service/openai)

Keywords: openai-api, openai-compatible, chat-completions, rest-api, ai-api, developer-tools, aichat, aiimage, AI API, REST API, Developer API, Ace Data Cloud

## Why Use OpenAI generation on Ace Data Cloud

- Unified developer platform with one API key, billing system, and usage tracking
- Production-ready AI API endpoints served from [https://api.acedata.cloud](https://api.acedata.cloud)
- English integration guides, API references, and service documentation
- Global-ready workflow for developers building chat, image, video, music, and search products

## Overview

<style>
.openai-page * { box-sizing: border-box; }
.openai-page h1, .openai-page h2, .openai-page h3, .openai-page h4, .openai-page h5, .openai-page h6, .openai-page p, .openai-page ul, .openai-page ol, .openai-page li, .openai-page pre, .openai-page blockquote, .openai-page table, .openai-page td, .openai-page th { margin: 0; padding: 0; }
.openai-page {
-webkit-font-smoothing: antialiased;
-moz-osx-font-smoothing: grayscale;
color: var(--el-text-color-primary);
background: var(--el-bg-color);
line-height: 1.6;
}
.openai-page a { text-decoration: none; color: inherit; }
.openai-page a:hover { text-decoration: none; }
.openai-page ul { list-style: none; }
.markdown-body .openai-page a { color: inherit !important; text-decoration: none !important; }
.markdown-body .openai-page a:hover { text-decoration: none !important; }
.markdown-body .openai-page a.oa-btn-primary,
.markdown-body .openai-page a.price-btn-fill,
.markdown-body .openai-page a.btn-cta-light { color: #ffffff !important; }
.markdown-body .openai-page a.oa-btn-secondary { color: var(--el-text-color-primary) !important; }
.markdown-body .openai-page a.price-btn-out { color: var(--el-text-color-primary) !important; }
.markdown-body .openai-page a.btn-cta-ghost { color: #94a3b8 !important; }
.markdown-body .openai-page a.btn-cta-ghost:hover { color: #e2e8f0 !important; }
.markdown-body .openai-page h1, .markdown-body .openai-page h2 { border-bottom: none !important; padding-bottom: 0 !important; }
.oa-container { max-width: 1200px; margin: 0 auto; padding: 0 24px; }
.oa-container-narrow { max-width: 800px; margin: 0 auto; padding: 0 24px; }
.oa-section { padding: 80px 0; }
.oa-section-sm { padding: 48px 0; }
.oa-bg-white { background: var(--el-bg-color); }
.oa-bg-gray { background: var(--el-bg-color-page); }
.oa-header { text-align: center; margin-bottom: 64px; }
.oa-header h2 {
font-size: clamp(28px, 4vw, 40px);
font-weight: 700;
color: var(--el-text-color-primary);
letter-spacing: normal;
margin-bottom: 20px;
line-height: 1.15;
}
.oa-header p {
font-size: clamp(16px, 2vw, 18px);
color: var(--el-text-color-regular);
max-width: 640px;
margin: 0 auto;
line-height: 1.6;
}
.openai-page .oa-btn-primary {
display: inline-flex; align-items: center; gap: 6px;
padding: 14px 28px;
background: #10a37f; color: #ffffff !important;
border-radius: 9999px; font-size: 15px; font-weight: 600;
transition: background 0.2s, transform 0.15s;
border: none; cursor: pointer;
text-decoration: none !important;
}
.openai-page .oa-btn-primary:hover { background: #0d8c6c; transform: translateY(-1px); text-decoration: none !important; }
.openai-page .oa-btn-secondary {
display: inline-flex; align-items: center; gap: 6px;
padding: 14px 28px;
background: var(--el-bg-color); color: var(--el-text-color-primary) !important;
border: 1px solid var(--el-border-color-light);
border-radius: 9999px; font-size: 15px; font-weight: 600;
transition: border-color 0.2s, background 0.2s;
cursor: pointer;
text-decoration: none !important;
}
.openai-page .oa-btn-secondary:hover { background: var(--el-bg-color-page); text-decoration: none !important; }
.openai-hero {
padding: 100px 0 80px;
text-align: center;
background: var(--el-bg-color);
position: relative;
overflow: hidden;
}
.openai-hero::before {
content: '';
position: absolute;
top: -200px; left: 50%;
transform: translateX(-50%);
width: 900px; height: 500px;
background: radial-gradient(ellipse, rgba(16, 163, 127, 0.06) 0%, transparent 70%);
pointer-events: none;
}
.oa-badge {
display: inline-flex; align-items: center; gap: 8px;
padding: 6px 16px;
background: var(--el-bg-color-page); border: 1px solid var(--el-border-color-light);
border-radius: 9999px; font-size: 13px; font-weight: 600; color: var(--el-text-color-regular);
margin-bottom: 28px;
}
.oa-badge-dot {
width: 6px; height: 6px; background: #10b981; border-radius: 50%;
display: inline-block;
}
.openai-hero h1 {
font-size: clamp(36px, 5vw, 60px);
line-height: 1.08;
letter-spacing: normal;
margin-bottom: 18px;
font-weight: 700;
}
.openai-hero h1 .oa-brand { color: #10a37f; }
.openai-hero h1 .oa-sub { color: var(--el-text-color-primary); }
.openai-page .hero-subtitle {
max-width: 680px;
margin: 0 auto;
font-size: clamp(16px, 2.1vw, 20px);
color: var(--el-text-color-regular);
line-height: 1.6;
}
.oa-actions {
margin-top: 56px;
display: flex; gap: 12px;
justify-content: center;
flex-wrap: wrap;
}
.oa-highlights {
display: flex; gap: 24px; justify-content: center; flex-wrap: wrap;
margin-top: 40px;
}
.oa-highlights .h-item { font-size: 13px; color: var(--el-text-color-regular); display: flex; align-items: center; gap: 6px; }
.oa-highlights .h-div { width: 1px; height: 14px; background: var(--el-border-color-light); }
.oa-stats {
display: grid !important;
grid-template-columns: repeat(4, 1fr) !important;
gap: 20px !important;
}
.oa-stat {
background: var(--el-bg-color);
border: 1px solid var(--el-border-color-light);
border-radius: 16px;
padding: 24px 16px;
text-align: center;
}
.oa-stat-val {
font-size: clamp(28px, 4vw, 38px);
font-weight: 700;
color: var(--el-text-color-primary);
line-height: 1.1;
}
.oa-stat-lbl { margin-top: 8px; font-size: 13px; color: var(--el-text-color-regular); }
.oa-feat-grid {
display: grid !important;
grid-template-columns: repeat(2, 1fr) !important;
gap: 20px !important;
}
.oa-feat-card {
background: var(--el-bg-color);
border: 1px solid var(--el-border-color-light);
border-radius: 16px;
padding: 28px 24px;
transition: border-color 0.2s;
}
.oa-feat-card:hover { border-color: var(--el-text-color-regular); }
.oa-feat-card h3 { font-size: 18px; font-weight: 700; color: var(--el-text-color-primary); margin-bottom: 8px; }
.oa-feat-card p { font-size: 15px; color: var(--el-text-color-regular); line-height: 1.6; }
.oa-feat-icon { font-size: 28px; margin-bottom: 12px; }
.oa-code-wrap {
border-radius: 16px !important;
overflow: hidden !important;
border: 1px solid #334155 !important;
background: #0f172a !important;
}
.markdown-body .openai-page .oa-code-wrap { background: #0f172a !important; }
.oa-code-head {
padding: 10px 16px;
background: #1e293b;
border-bottom: 1px solid #334155;
color: #cbd5e1;
font-size: 12px;
letter-spacing: 0.06em;
text-transform: uppercase;
}
.oa-code
{
margin: 0 !important;
padding: 18px !important;
white-space: pre !important;
overflow-x: auto !important;
font-family: 'JetBrains Mono', 'Fira Code', 'SFMono-Regular', monospace !important;
font-size: 13px !important;
line-height: 1.7 !important;
color: #e2e8f0 !important;
background: transparent !important;
border: none !important;
}
.markdown-body .openai-page .oa-code { background: transparent !important; }
.oa-code-grid {
display: grid !important;
grid-template-columns: 1fr 1fr !important;
gap: 0 !important;
align-items: stretch;
}
.oa-code-left {
display: flex; flex-direction: column; justify-content: center;
padding: 0 40px 0 0;
}
.oa-code-left h2 {
font-size: clamp(24px, 3.2vw, 34px);
font-weight: 700;
color: var(--el-text-color-primary);
margin-bottom: 16px;
}
.oa-code-left > p {
font-size: 16px; color: var(--el-text-color-regular); line-height: 1.6;
}
.oa-steps {
display: grid !important;
grid-template-columns: repeat(3, 1fr) !important;
gap: 0 !important;
position: relative;
}
.oa-step { text-align: center; padding: 0 24px; }
.oa-step-num {
width: 44px; height: 44px;
border-radius: 50%;
background: var(--el-bg-color-page);
border: 2px solid var(--el-border-color-light);
display: inline-flex; align-items: center; justify-content: center;
font-size: 16px; font-weight: 700; color: var(--el-text-color-regular);
margin-bottom: 16px;
}
.oa-step h3 { font-size: 17px; font-weight: 700; color: var(--el-text-color-primary); margin-bottom: 8px; }
.oa-step p { font-size: 14px; color: var(--el-text-color-regular); line-height: 1.6; }
.oa-step-conn {
position: absolute;
top: 22px;
height: 2px;
background: var(--el-border-color-light);
}
.oa-step-conn-1 { left: calc(100% / 6); right: calc(100% / 6 * 3); }
.oa-step-conn-2 { left: calc(100% / 6 * 3); right: calc(100% / 6); }
.oa-uc-grid {
display: grid !important;
grid-template-columns: repeat(3, 1fr) !important;
gap: 20px !important;
}
.oa-uc-card {
background: var(--el-bg-color);
border: 1px solid var(--el-border-color-light);
border-radius: 16px;
padding: 24px;
transition: border-color 0.2s;
}
.oa-uc-card:hover { border-color: var(--el-text-color-regular); }
.oa-uc-icon { font-size: 28px; margin-bottom: 10px; }
.oa-uc-card h3 { font-size: 16px; font-weight: 700; color: var(--el-text-color-primary); margin-bottom: 6px; }
.oa-uc-card p { font-size: 14px; color: var(--el-text-color-regular); line-height: 1.5; }
.oa-api-grid {
display: grid !important;
grid-template-columns: repeat(2, 1fr) !important;
gap: 20px !important;
}
.oa-api-card {
background: var(--el-bg-color);
border: 1px solid var(--el-border-color-light);
border-radius: 16px;
padding: 28px 24px;
transition: border-color 0.2s;
}
.oa-api-card:hover { border-color: var(--el-text-color-regular); }
.oa-api-card h3 { font-size: 18px; font-weight: 700; color: var(--el-text-color-primary); margin-bottom: 6px; }
.openai-page .oa-api-card .api-desc { font-size: 14px; color: var(--el-text-color-regular); margin-bottom: 12px; line-height: 1.5; }
.oa-api-path {
display: inline-block;
padding: 3px 10px;
background: var(--el-bg-color-page);
border-radius: 6px;
font-size: 12px;
color: var(--el-text-color-regular);
font-family: 'JetBrains Mono', monospace;
}
.oa-mdl-grid {
display: grid !important;
grid-template-columns: repeat(3, 1fr) !important;
gap: 20px !important;
}
.oa-mdl-card {
background: var(--el-bg-color);
border: 1px solid var(--el-border-color-light);
border-radius: 16px;
padding: 24px;
}
.oa-mdl-card h3 { font-size: 16px; font-weight: 700; color: var(--el-text-color-primary); margin-bottom: 4px; }
.openai-page .oa-mdl-card .mdl-desc { font-size: 13px; color: var(--el-text-color-regular); margin-bottom: 10px; line-height: 1.5; }
.oa-mdl-tags { display: flex; flex-wrap: wrap; gap: 5px; }
.oa-mdl-tag {
padding: 2px 8px;
background: var(--el-bg-color-page);
border-radius: 6px;
font-size: 11px;
color: var(--el-text-color-regular);
font-family: 'JetBrains Mono', monospace;
}
.price-grid {
display: grid !important;
grid-template-columns: repeat(2, 1fr) !important;
gap: 20px !important;
max-width: 860px;
margin: 0 auto;
}
.price-card {
background: var(--el-bg-color);
border: 1px solid var(--el-border-color-light);
border-radius: 20px;
padding: 36px 28px;
text-align: center;
}
.price-card-feat {
border-color: #10a37f;
position: relative;
}
.price-feat-badge {
position: absolute;
top: -12px; left: 50%; transform: translateX(-50%);
padding: 4px 16px;
background: #10a37f; color: #fff;
border-radius: 9999px; font-size: 12px; font-weight: 700;
}
.price-tier {
font-size: 16px; font-weight: 700; color: var(--el-text-color-primary);
margin: 16px 0 8px;
}
.price-amt {
font-size: clamp(32px, 4vw, 44px);
font-weight: 800;
color: var(--el-text-color-primary);
}
.price-per { font-size: 14px; color: var(--el-text-color-regular); }
.openai-page .price-card .price-desc {
font-size: 14px;
color: var(--el-text-color-regular);
margin: 12px 0 20px;
line-height: 1.5;
}
.openai-page .price-card .price-feats {
text-align: left;
margin-bottom: 24px;
}
.openai-page .price-card .price-feats li {
font-size: 14px;
color: var(--el-text-color-regular);
padding: 6px 0;
display: flex;
align-items: center;
gap: 8px;
}
.price-ck { color: #10b981; font-size: 14px; flex-shrink: 0; }
.price-btn {
display: inline-block;
width: 100%;
padding: 12px 0;
border-radius: 10px;
font-size: 14px;
font-weight: 600;
text-align: center;
transition: all 0.2s;
}
.price-btn-fill {
background: #10a37f; color: #fff !important;
}
.price-btn-fill:hover { background: #0d8c6c; }
.price-btn-out {
background: transparent;
border: 1px solid var(--el-border-color-light);
color: var(--el-text-color-primary) !important;
}
.price-btn-out:hover { border-color: var(--el-text-color-regular); }
.oa-img-price-wrap {
max-width: 700px;
margin: 48px auto 0;
border-radius: 16px;
overflow: hidden;
border: 1px solid var(--el-border-color-light);
}
.oa-img-price-title {
padding: 14px 20px;
background: var(--el-bg-color-page);
font-size: 15px;
font-weight: 700;
color: var(--el-text-color-primary);
border-bottom: 1px solid var(--el-border-color-light);
}
.oa-img-price-table {
display: table !important;
width: 100% !important;
border-collapse: collapse !important;
}
.oa-img-price-table th
{
padding: 12px 16px !important;
background: var(--el-bg-color-page) !important;
font-size: 12px !important;
font-weight: 700 !important;
color: var(--el-text-color-regular) !important;
text-transform: uppercase !important;
letter-spacing: 0.06em !important;
text-align: left !important;
border: none !important;
border-bottom: 1px solid var(--el-border-color-light) !important;
}
.oa-img-price-table td {
padding: 10px 16px !important;
font-size: 14px !important;
color: var(--el-text-color-primary) !important;
border: none !important;
border-bottom: 1px solid var(--el-border-color-lighter) !important;
background: var(--el-bg-color) !important;
}
.oa-img-price-table tr:last-child td { border-bottom: none !important; }
.oa-img-price-table .td-price {
color: #10a37f !important;
font-weight: 700 !important;
font-family: 'JetBrains Mono', monospace !important;
}
.oa-faq-list { max-width: 800px; margin: 0 auto; }
.oa-faq-item { border-bottom: 1px solid var(--el-border-color-lighter); }
.oa-faq-q {
display: flex; justify-content: space-between; align-items: center;
padding: 20px 0; cursor: pointer;
font-size: 16px; font-weight: 600; color: var(--el-text-color-primary);
transition: color 0.2s;
}
.oa-faq-q:hover { color: #10a37f; }
.oa-faq-chev { font-size: 18px; color: var(--el-text-color-regular); transition: transform 0.2s; }
.oa-faq-a { display: none; padding: 0 0 20px; }
.oa-faq-a p { font-size: 15px; color: var(--el-text-color-regular); line-height: 1.7; }
.oa-rel-grid {
display: grid !important;
grid-template-columns: repeat(4, 1fr) !important;
gap: 16px !important;
}
.oa-rel-card {
display: flex; align-items: center; gap: 14px;
background: var(--el-bg-color);
border: 1px solid var(--el-border-color-light);
border-radius: 14px;
padding: 16px 18px;
transition: border-color 0.2s;
}
.oa-rel-card:hover { border-color: var(--el-text-color-regular); }
.oa-rel-icon { font-size: 28px; flex-shrink: 0; }
.oa-rel-info h3 { font-size: 15px; font-weight: 700; color: var(--el-text-color-primary); }
.oa-rel-info p { font-size: 12px; color: var(--el-text-color-regular); margin-top: 2px; }
.oa-rel-arrow { margin-left: auto; font-size: 16px; color: var(--el-border-color); transition: color 0.2s; }
.oa-rel-card:hover .oa-rel-arrow { color: var(--el-text-color-regular); }
.openai-cta {
text-align: center;
background: #0f172a;
color: #f8fafc;
padding: 80px 0;
position: relative;
overflow: hidden;
}
.openai-cta::before {
content: '';
position: absolute;
top: -100px; left: 50%;
transform: translateX(-50%);
width: 600px; height: 400px;
background: radial-gradient(ellipse, rgba(16, 163, 127, 0.15) 0%, transparent 70%);
pointer-events: none;
}
.openai-cta h2 {
font-size: clamp(30px, 4.2vw, 46px);
font-weight: 700;
letter-spacing: normal;
margin-bottom: 28px;
position: relative;
}
.openai-cta p {
font-size: 17px;
color: #cbd5e1;
max-width: 600px;
margin: 0 auto 56px;
position: relative;
}
.openai-cta .oa-actions { position: relative; margin-top: 0; }
.openai-page .btn-cta-light {
display: inline-flex; align-items: center; gap: 6px;
padding: 14px 28px;
background: #10a37f; color: #ffffff !important;
border-radius: 9999px; font-size: 15px; font-weight: 600;
transition: background 0.2s;
text-decoration: none !important;
}
.openai-page .btn-cta-light:hover { background: #0d8c6c; }
.openai-page .btn-cta-ghost {
display: inline-flex; align-items: center; gap: 6px;
padding: 14px 28px;
background: transparent; color: #94a3b8 !important;
border: 1px solid #475569;
border-radius: 9999px; font-size: 15px; font-weight: 600;
transition: border-color 0.2s, color 0.2s;
text-decoration: none !important;
}
.openai-page .btn-cta-ghost:hover { border-color: #94a3b8; color: #e2e8f0 !important; }
.openai-page code
{
background: #ecfdf5 !important; color: #059669 !important;
border: 1px solid #a7f3d0 !important;
padding: 2px 6px !important; border-radius: 4px !important;
font-size: 0.88em !important; font-weight: 600 !important;
}
html.dark .openai-page { background: var(--el-bg-color); color: var(--el-text-color-primary); }
html.dark .markdown-body .openai-page a { color: inherit !important; text-decoration: none !important; }
html.dark .markdown-body .openai-page a.oa-btn-primary,
html.dark .markdown-body .openai-page a.price-btn-fill,
html.dark .markdown-body .openai-page a.btn-cta-light { color: #ffffff !important; }
html.dark .markdown-body .openai-page a.oa-btn-secondary { color: var(--el-text-color-primary) !important; }
html.dark .markdown-body .openai-page a.price-btn-out { color: var(--el-text-color-primary) !important; }
html.dark .markdown-body .openai-page a.btn-cta-ghost { color: #94a3b8 !important; }
html.dark .markdown-body .openai-page a.btn-cta-ghost:hover { color: var(--el-text-color-primary) !important; }
html.dark .oa-bg-white { background: var(--el-bg-color); }
html.dark .oa-bg-gray { background: var(--el-bg-color-page); }
html.dark .oa-header h2 { color: var(--el-text-color-primary); }
html.dark .oa-header p { color: var(--el-text-color-secondary); }
html.dark .openai-page .oa-btn-primary { background: #10a37f; }
html.dark .openai-page .oa-btn-primary:hover { background: #0d8c6c; }
html.dark .openai-page .oa-btn-secondary { background: #1e293b; color: var(--el-text-color-primary) !important; border-color: #475569; }
html.dark .openai-page .oa-btn-secondary:hover { background: var(--el-border-color); border-color: var(--el-text-color-regular); }
html.dark .openai-hero { background: var(--el-bg-color); }
html.dark .openai-hero::before { background: radial-gradient(ellipse, rgba(16, 163, 127, 0.15) 0%, transparent 70%); }
html.dark .oa-badge { background: var(--el-bg-color-page); border-color: var(--el-border-color); color: var(--el-text-color-secondary); }
html.dark .openai-hero h1 .oa-brand { color: #34d399; }
html.dark .openai-hero h1 .oa-sub { color: var(--el-text-color-primary); }
html.dark .openai-page .hero-subtitle { color: var(--el-text-color-secondary); }
html.dark .oa-highlights .h-item { color: var(--el-text-color-secondary); }
html.dark .oa-highlights .h-div { background: var(--el-border-color); }
html.dark .oa-stat { background: var(--el-bg-color-page); border-color: var(--el-border-color); box-shadow: none; }
html.dark .oa-stat-val { color: var(--el-text-color-primary); }
html.dark .oa-stat-lbl { color: var(--el-text-color-regular); }
html.dark .oa-feat-card { background: var(--el-bg-color-page); border-color: var(--el-border-color); }
html.dark .oa-feat-card:hover { border-color: var(--el-text-color-regular); }
html.dark .oa-feat-card h3 { color: var(--el-text-color-primary); }
html.dark .oa-feat-card p { color: var(--el-text-color-secondary); }
html.dark .oa-code-left h2 { color: var(--el-text-color-primary); }
html.dark .oa-code-left > p { color: var(--el-text-color-secondary); }
html.dark .oa-step-num { background: var(--el-border-color); border-color: var(--el-text-color-regular); color: var(--el-text-color-secondary); }
html.dark .oa-step h3 { color: var(--el-text-color-primary); }
html.dark .oa-step p { color: var(--el-text-color-regular); }
html.dark .oa-step-conn { background: var(--el-border-color); }
html.dark .oa-uc-card { background: var(--el-bg-color-page); border-color: var(--el-border-color); }
html.dark .oa-uc-card:hover { border-color: var(--el-text-color-regular); }
html.dark .oa-uc-card h3 { color: var(--el-text-color-primary); }
html.dark .oa-uc-card p { color: var(--el-text-color-secondary); }
html.dark .oa-api-card { background: var(--el-bg-color-page); border-color: var(--el-border-color); }
html.dark .oa-api-card:hover { border-color: var(--el-text-color-regular); }
html.dark .oa-api-card h3 { color: var(--el-text-color-primary); }
html.dark .openai-page .oa-api-card .api-desc { color: var(--el-text-color-secondary); }
html.dark .oa-api-path { background: var(--el-border-color); color: var(--el-text-color-secondary); }
html.dark .oa-mdl-card { background: var(--el-bg-color-page); border-color: var(--el-border-color); }
html.dark .oa-mdl-card h3 { color: var(--el-text-color-primary); }
html.dark .openai-page .oa-mdl-card .mdl-desc { color: var(--el-text-color-secondary); }
html.dark .oa-mdl-tag { background: var(--el-border-color); color: var(--el-text-color-secondary); }
html.dark .price-card { background: var(--el-bg-color-page); border-color: var(--el-border-color); box-shadow: none; }
html.dark .price-card-feat { border-color: #10a37f; }
html.dark .price-tier { color: var(--el-text-color-primary); }
html.dark .price-amt { color: var(--el-text-color-primary); }
html.dark .openai-page .price-card .price-desc { color: var(--el-text-color-secondary); }
html.dark .openai-page .price-card .price-feats li { color: var(--el-text-color-secondary); }
html.dark .price-btn-out { border-color: var(--el-border-color); }
html.dark .oa-img-price-wrap { border-color: var(--el-border-color); }
html.dark .oa-img-price-title { background: var(--el-bg-color-page); color: var(--el-text-color-primary); border-bottom-color: var(--el-border-color); }
html.dark .oa-img-price-table th { background: var(--el-bg-color-page) !important; color: var(--el-text-color-secondary) !important; border-bottom-color: var(--el-border-color) !important; }
html.dark .oa-img-price-table td { color: var(--el-text-color-primary) !important; border-bottom-color: var(--el-border-color) !important; background: var(--el-bg-color) !important; }
html.dark .oa-img-price-table .td-price { color: #34d399 !important; }
html.dark .oa-faq-item { border-color: var(--el-border-color); }
html.dark .oa-faq-q { color: var(--el-text-color-primary); }
html.dark .oa-faq-q:hover { color: #34d399; }
html.dark .oa-faq-chev { color: var(--el-text-color-regular); }
html.dark .oa-faq-a p { color: var(--el-text-color-secondary); }
html.dark .oa-rel-card { background: var(--el-bg-color-page); border-color: var(--el-border-color); }
html.dark .oa-rel-card:hover { border-color: var(--el-text-color-regular); }
html.dark .oa-rel-info h3 { color: var(--el-text-color-primary); }
html.dark .oa-rel-info p { color: var(--el-text-color-regular); }
html.dark .oa-rel-arrow { color: var(--el-text-color-regular); }
html.dark .openai-cta { background: #020617; }
html.dark .openai-cta::before { background: radial-gradient(ellipse, rgba(16, 163, 127, 0.2) 0%, transparent 70%); }
html.dark .openai-page .btn-cta-light { color: #ffffff !important; }
html.dark .openai-page .btn-cta-ghost { color: #94a3b8 !important; }
html.dark .openai-page .btn-cta-ghost:hover { color: var(--el-text-color-primary) !important; }
html.dark .openai-page code { background: #064e3b !important; color: #6ee7b7 !important; border-color: #059669 !important; }
@media (max-width: 980px)
{
.oa-stats { grid-template-columns: repeat(2, 1fr) !important; }
.oa-feat-grid { grid-template-columns: 1fr !important; }
.oa-code-grid { grid-template-columns: 1fr !important; }
.oa-code-left { padding: 0 0 32px 0; }
.oa-steps { grid-template-columns: 1fr !important; gap: 24px !important; }
.oa-step-conn { display: none; }
.oa-uc-grid { grid-template-columns: 1fr !important; }
.oa-api-grid { grid-template-columns: 1fr !important; }
.oa-mdl-grid { grid-template-columns: 1fr !important; }
.price-grid { grid-template-columns: 1fr !important; }
.oa-rel-grid { grid-template-columns: repeat(2, 1fr) !important; }
}
@media (max-width: 480px) {
.oa-stats { grid-template-columns: 1fr !important; }
.oa-rel-grid { grid-template-columns: 1fr !important; }
} </style>
<div class="openai-page">
<section class="openai-hero">
<div class="oa-container">
<div class="oa-badge"><span class="oa-badge-dot"></span>OpenAI · GPT / o-series / DALL·E</div>
<h1><span class="oa-brand">OpenAI</span> <span class="oa-sub">Full Series API</span></h1>
<p class="hero-subtitle">Access the full range of OpenAI models through a unified interface—GPT-5, GPT-4o, o3, o1 conversational reasoning, DALL·E 3 / GPT Image image generation, and Embeddings vectorization.</p>
<div class="oa-actions">
<a class="oa-btn-primary" href="/apis/openai-chat-completions">📄 View Documentation</a>
<a class="oa-btn-secondary" href="/apis/openai-images-generations">🎨 Image Generation</a>
</div>
<div class="oa-highlights">
<span class="h-item">🧠 GPT-5 / GPT-4o</span>
<span class="h-div"></span>
<span class="h-item">💡 o3 / o4-mini reasoning</span>
<span class="h-div"></span>
<span class="h-item">🎨 DALL·E 3 / GPT Image</span>
<span class="h-div"></span>
<span class="h-item">📐 Embeddings</span>
</div>
</div>
</section>
<section class="oa-section-sm oa-bg-gray">
<div class="oa-container">
<div class="oa-stats">
<div class="oa-stat"><div class="oa-stat-val">25+</div><div class="oa-stat-lbl">Available Models</div></div>
<div class="oa-stat"><div class="oa-stat-val">5</div><div class="oa-stat-lbl">API Endpoints</div></div>
<div class="oa-stat"><div class="oa-stat-val">Token</div><div class="oa-stat-lbl">Pay-as-you-go</div></div>
<div class="oa-stat"><div class="oa-stat-val">∞</div><div class="oa-stat-lbl">No Rate Limits</div></div>
</div>
</div>
</section>
<section class="oa-section oa-bg-white">
<div class="oa-container">
<div class="oa-header">
<h2>Covering All Capabilities of OpenAI</h2>
<p>Conversational reasoning, image generation, image editing, text vectorization—one API Key does it all</p>
</div>
<div class="oa-feat-grid">
<div class="oa-feat-card">
<div class="oa-feat-icon">💬</div>
<h3>Chat Completions</h3>
<p>Fully supports GPT-5, GPT-4o, GPT-4.1 series, and o series reasoning models, with streaming output, tool invocation, and full support for JSON Mode.</p>
</div>
<div class="oa-feat-card">
<div class="oa-feat-icon">🧠</div>
<h3>Responses API</h3>
<p>OpenAI's latest Responses API format, supporting the same models as Chat Completions, providing a more flexible interaction method.</p>
</div>
<div class="oa-feat-card">
<div class="oa-feat-icon">🎨</div>
<h3>Image Generation / Editing</h3>
<p>DALL·E 3, DALL·E 2, and GPT Image 1 / 1.5 support text-to-image and image editing, with multiple size and quality options.</p>
</div>
<div class="oa-feat-card">
<div class="oa-feat-icon">📐</div>
<h3>Embeddings</h3>
<p>Three embedding models: text-embedding-3-small/large and ada-002, for semantic search, clustering, and RAG applications.</p>
</div>
</div>
</div>
</section>
<section class="oa-section oa-bg-gray">
<div class="oa-container">
<div class="oa-code-grid">
<div class="oa-code-left">
<h2>Native OpenAI SDK Direct Connection</h2>
<p>Modify base_url to access the full range of OpenAI models through Ace Data Cloud, fully compatible with the official SDK.</p>
</div>
<div>
<div class="oa-code-wrap">
<div class="oa-code-head">Python</div>
<pre class="oa-code">from openai import OpenAI
 

client = OpenAI(
base_url="https://api.acedata.cloud",
api_key="YOUR_API_KEY"
)

# Chat Completions

response = client.chat.completions.create(
model="gpt-5",
messages=[
{"role": "user", "content": "Explain the core concepts of machine learning"}
]
)
print(response.choices[0].message.content)

# Image Generation

image = client.images.generate(
model="gpt-image-1",
prompt="A futuristic city at sunset",
n=1
)
print(image.data[0].url)</pre>
</div>
</div>
</div>
</div>
</section>
<section class="oa-section oa-bg-white">
<div class="oa-container">
<div class="oa-header">
<h2>3 Steps to Get Started Quickly</h2>
<p>It only takes a few minutes from registration to completing your first API call.</p>
</div>
<div class="oa-steps">
<div class="oa-step-conn oa-step-conn-1"></div>
<div class="oa-step-conn oa-step-conn-2"></div>
<div class="oa-step">
<div class="oa-step-num">1</div>
<h3>Get API Key</h3>
<p>Register for Ace Data Cloud and generate an API key from the console.</p>
</div>
<div class="oa-step">
<div class="oa-step-num">2</div>
<h3>Select API and Model</h3>
<p>Choose the Chat, Images, or Embeddings API based on your needs, along with the required model.</p>
</div>
<div class="oa-step">
<div class="oa-step-num">3</div>
<h3>Make a Request</h3>
<p>Use the OpenAI SDK or HTTP requests, just modify the base_url to call.</p>
</div>
</div>
</div>
</section>
<section class="oa-section oa-bg-gray">
<div class="oa-container">
<div class="oa-header">
<h2>What is the OpenAI API Suitable For?</h2>
<p>Covers various AI application scenarios including text, images, and vectorization.</p>
</div>
<div class="oa-uc-grid">
<div class="oa-uc-card">
<div class="oa-uc-icon">💻</div>
<h3>Code Assistant</h3>
<p>GPT-5 and o3 excel in programming tasks, suitable for code generation, review, and debugging.</p>
</div>
<div class="oa-uc-card">
<div class="oa-uc-icon">🤖</div>
<h3>AI Customer Service / Chatbot</h3>
<p>Build intelligent customer service systems, GPT-4o-mini provides cost-effective conversational capabilities.</p>
</div>
<div class="oa-uc-card">
<div class="oa-uc-icon">🎨</div>
<h3>AI Painting</h3>
<p>DALL·E 3 and GPT Image generate high-quality images, suitable for design and marketing material creation.</p>
</div>
<div class="oa-uc-card">
<div class="oa-uc-icon">🔍</div>
<h3>Semantic Search / RAG</h3>
<p>Embeddings API vectorizes text to build semantic search engines and knowledge base Q&A systems.</p>
</div>
<div class="oa-uc-card">
<div class="oa-uc-icon">📊</div>
<h3>Data Analysis</h3>
<p>Input data for GPT to analyze trends, generate reports, and visualize suggestions.</p>
</div>
<div class="oa-uc-card">
<div class="oa-uc-icon">🔬</div>
<h3>Complex Reasoning</h3>
<p>Models like o3 and o3-pro excel in mathematical proofs, logical analysis, and multi-step problem solving.</p>
</div>
</div>
</div>
</section>
<section class="oa-section oa-bg-white">
<div class="oa-container">
<div class="oa-header">
<h2>API Endpoints</h2>
<p>5 API endpoints cover the full capabilities of OpenAI</p>
</div>
<div class="oa-api-grid">
<div class="oa-api-card">
<h3>💬 Chat Completions</h3>
<p class="api-desc">Conversational text generation, supports streaming output, tool calls, etc.</p>
<span class="oa-api-path">/openai/chat/completions</span>
</div>
<div class="oa-api-card">
<h3>🧠 Responses</h3>
<p class="api-desc">OpenAI's next-generation response interface, supports the same model set.</p>
<span class="oa-api-path">/openai/responses</span>
</div>
<div class="oa-api-card">
<h3>🎨 Images Generations</h3>
<p class="api-desc">Text to image generation, DALL·E 3 / GPT Image.</p>
<span class="oa-api-path">/openai/images/generations</span>
</div>
<div class="oa-api-card">
<h3>✏️ Images Edits</h3>
<p class="api-desc">Image editing based on image and text prompts.</p>
<span class="oa-api-path">/openai/images/edits</span>
</div>
<div class="oa-api-card">
<h3>📐 Embeddings</h3>
<p class="api-desc">Text vectorization for search, clustering, and RAG.</p>
<span class="oa-api-path">/openai/embeddings</span>
</div>
</div>
</div>
</section>
<section class="oa-section oa-bg-gray">
<div class="oa-container">
<div class="oa-header">
<h2>Model Matrix</h2>
<p>Select the most suitable model by task</p>
</div>
<div class="oa-mdl-grid">
<div class="oa-mdl-card">
<h3>🚀 GPT-5 Series</h3>
<p class="mdl-desc">The latest flagship conversational model, significantly enhanced reasoning and creative capabilities.</p>
<div class="oa-mdl-tags">
<span class="oa-mdl-tag">gpt-5</span>
<span class="oa-mdl-tag">gpt-5-mini</span>
<span class="oa-mdl-tag">gpt-5-nano</span>
<span class="oa-mdl-tag">gpt-5-pro</span>
</div>
</div>
<div class="oa-mdl-card">
<h3>✨ GPT-5.x Series</h3>
<p class="mdl-desc">Enhanced versions of GPT-5, 5.1 and 5.2 further improve capabilities.</p>
<div class="oa-mdl-tags">
<span class="oa-mdl-tag">gpt-5.1</span>
<span class="oa-mdl-tag">gpt-5.2</span>
</div>
</div>
<div class="oa-mdl-card">
<h3>⚡ GPT-4o Series</h3>
<p class="mdl-desc">Efficient multimodal model, supports mixed input of text and images.</p>
<div class="oa-mdl-tags">
<span class="oa-mdl-tag">gpt-4o</span>
<span class="oa-mdl-tag">gpt-4o-mini</span>
<span class="oa-mdl-tag">gpt-4o-image</span>
</div>
</div>
<div class="oa-mdl-card">
<h3>📐 GPT-4.1 Series</h3>
<p class="mdl-desc">Optimized for programming and long context, strong instruction-following capabilities.</p>
<div class="oa-mdl-tags">
<span class="oa-mdl-tag">gpt-4.1</span>
<span class="oa-mdl-tag">gpt-4.1-mini</span>
<span class="oa-mdl-tag">gpt-4.1-nano</span>
</div>
</div>
<div class="oa-mdl-card">
<h3>🧠 o Series Reasoning</h3>
<p class="mdl-desc">Designed for complex reasoning, provides high-quality answers after deep thinking.</p>
<div class="oa-mdl-tags">
<span class="oa-mdl-tag">o3</span>
<span class="oa-mdl-tag">o3-pro</span>
<span class="oa-mdl-tag">o3-mini</span>
<span class="oa-mdl-tag">o4-mini</span>
<span class="oa-mdl-tag">o1</span>
<span class="oa-mdl-tag">o1-pro</span>
</div>
</div>
<div class="oa-mdl-card">
<h3>🎨 Image Models</h3>
<p class="mdl-desc">Text-to-image generation and image editing.</p>
<div class="oa-mdl-tags">
<span class="oa-mdl-tag">gpt-image-1</span>
<span class="oa-mdl-tag">gpt-image-1.5</span>
<span class="oa-mdl-tag">dall-e-3</span>
<span class="oa-mdl-tag">dall-e-2</span>
</div>
</div>
</div>
</div>
</section>
<section class="oa-section oa-bg-white">
<div class="oa-container">
<div class="oa-header">
<h2>OpenAI API Pricing</h2>
<p>Charged based on actual usage, conversation by Token, images by piece, embeddings by Token.</p>
<p style="font-size:14px;color:#94a3b8;margin-top:8px;">Bulk packages offer more discounts</p>
</div>
<div class="price-grid">
<div class="price-card price-card-feat">
<div class="price-feat-badge">Pay-as-you-go</div>
<div class="price-tier">Token / Per Image Billing</div>
<div>
<span class="price-amt">Low Price</span>
<span class="price-per"> Based on actual usage</span>
</div>
<p class="price-desc">Text billed by Token, images billed by piece, no minimum consumption</p>
<ul class="price-feats">
<li><span class="price-ck">✓</span> Full range of GPT-5 / 4o / 4.1</li>
<li><span class="price-ck">✓</span> o3 / o4-mini reasoning models</li>
<li><span class="price-ck">✓</span> Image generation starting at $0.019 / piece</li>
<li><span class="price-ck">✓</span> Embeddings as low as $0.0027 / million Tokens</li>
<li><span class="price-ck">✓</span> Streaming — <strong>Free</strong></li>
</ul>
<a href="https://platform.acedata.cloud/apis/openai-chat-completions" class="price-btn price-btn-fill">View Pricing Details</a>
</div>
<div class="price-card">
<div class="price-tier">Enterprise Edition</div>
<div>
<span class="price-amt">Custom</span>
</div>
<p class="price-desc">Dedicated plans for high-usage teams</p>
<ul class="price-feats">
<li><span class="price-ck">✓</span> Tiered discounts based on usage</li>
<li><span class="price-ck">✓</span> Priority support with account manager</li>
<li><span class="price-ck">✓</span> Custom rate limits</li>
<li><span class="price-ck">✓</span> SLA guarantees</li>
<li><span class="price-ck">✓</span> Private deployment options</li>
</ul>
<a href="https://platform.acedata.cloud/support" class="price-btn price-btn-out">Contact Sales</a>
</div>
</div>
<div class="oa-img-price-wrap">
<div class="oa-img-price-title">🎨 Reference Prices for Image Generation</div>
<table class="oa-img-price-table">
<thead>
<tr>
<th>Model</th>
<th>Specifications</th>
<th>Price per Image</th>
</tr>
</thead>
<tbody>
<tr>
<td>GPT Image 1</td>
<td>All Sizes</td>
<td class="td-price">$0.019</td>
</tr>
<tr>
<td>GPT Image 1.5</td>
<td>All Sizes</td>
<td class="td-price">$0.052</td>
</tr>
<tr>
<td>DALL·E 3</td>
<td>Standard 1024×1024</td>
<td class="td-price">$0.019</td>
</tr>
<tr>
<td>DALL·E 3</td>
<td>Standard 1024×1792</td>
<td class="td-price">$0.038</td>
</tr>
<tr>
<td>DALL·E 3</td>
<td>HD 1024×1024</td>
<td class="td-price">$0.038</td>
</tr>
<tr>
<td>DALL·E 3</td>
<td>HD 1024×1792</td>
<td class="td-price">$0.057</td>
</tr>
<tr>
<td>DALL·E 2</td>
<td>1024×1024</td>
<td class="td-price">$0.0095</td>
</tr>
</tbody>
</table>
</div>
</div>
</section>
<section class="oa-section oa-bg-gray">
<div class="oa-container-narrow">
<div class="oa-header">
<h2>Frequently Asked Questions</h2>
<p>Common questions about using the OpenAI API</p>
</div>
<div class="oa-faq-list">
<div class="oa-faq-item">
<div class="oa-faq-q"><span>What is the difference between using this and directly using the official OpenAI API?</span><span class="oa-faq-chev">›</span></div>
<div class="oa-faq-a"><p>Ace Data Cloud provides a proxy interface fully compatible with the official OpenAI format, without the need to register an OpenAI account or bind an overseas credit card. One API Key can access multiple models such as OpenAI, Claude, Gemini, Grok, etc.</p></div>
</div>
<div class="oa-faq-item">
<div class="oa-faq-q"><span>What is the difference between GPT-5 and GPT-4o?</span><span class="oa-faq-chev">›</span></div>
<div class="oa-faq-a"><p>GPT-5 is OpenAI's latest flagship model, surpassing GPT-4o in reasoning, creativity, and knowledge capabilities. GPT-4o is a well-validated cost-effective choice, suitable for cost-sensitive scenarios.</p></div>
</div>
<div class="oa-faq-item">
<div class="oa-faq-q"><span>What scenarios are the o series reasoning models suitable for?</span><span class="oa-faq-chev">›</span></div>
<div class="oa-faq-a"><p>Models like o3, o3-pro, and o4-mini perform deep thinking before providing answers, particularly suitable for tasks requiring deep reasoning such as mathematical calculations, logical reasoning, and code analysis. o3-mini and o4-mini offer lower-cost reasoning capabilities.</p></div>
</div>
<div class="oa-faq-item">
<div class="oa-faq-q"><span>What is the difference between GPT Image and DALL·E 3?</span><span class="oa-faq-chev">›</span></div>
<div class="oa-faq-a"><p>GPT Image 1 / 1.5 is OpenAI's latest native image generation model, directly integrated into the GPT model, supporting more natural text-image interaction. DALL·E 3 is a dedicated image generation model, supporting various quality and size options including Standard/HD.</p></div>
</div>
<div class="oa-faq-item">
<div class="oa-faq-q"><span>Which model is good for Embeddings?</span><span class="oa-faq-chev">›</span></div>
<div class="oa-faq-a"><p>It is recommended to use <code>text-embedding-3-small</code>, which has the best cost-performance ratio ($0.0027 / million Tokens). For higher precision, use <code>text-embedding-3-large</code> ($0.017 / million Tokens). <code>ada-002</code> is an older model, compatible but not as cost-effective as v3.</p></div>
</div>
</div>
</div>
</section>
<section class="oa-section oa-bg-white">
<div class="oa-container">
<div class="oa-header">
<h2>Explore More AI Models</h2>
<p>Ace Data Cloud offers a variety of large language models and AI service APIs</p>
</div>
<div class="oa-rel-grid">
<a class="oa-rel-card" href="/services/claude">
<span class="oa-rel-icon">🟠</span>
<div class="oa-rel-info"><h3>Claude</h3><p>Anthropic AI</p></div>
<span class="oa-rel-arrow">→</span>
</a>
<a class="oa-rel-card" href="/services/gemini">
<span class="oa-rel-icon">💎</span>
<div class="oa-rel-info"><h3>Gemini</h3><p>Google AI</p></div>
<span class="oa-rel-arrow">→</span>
</a>
<a class="oa-rel-card" href="/services/grok">
<span class="oa-rel-icon">🔴</span>
<div class="oa-rel-info"><h3>Grok</h3><p>xAI</p></div>
<span class="oa-rel-arrow">→</span>
</a>
<a class="oa-rel-card" href="/services/midjourney">
<span class="oa-rel-icon">🎨</span>
<div class="oa-rel-info"><h3>Midjourney</h3><p>AI Painting</p></div>
<span class="oa-rel-arrow">→</span>
</a>
</div>
</div>
</section>
<section class="openai-cta">
<div class="oa-container">
<h2>Start Using OpenAI API Now</h2>
<p>One API Key connects to the full range of OpenAI models, from conversation to images to vectorization.</p>
<div class="oa-actions">
<a class="btn-cta-light" href="/apis/openai-chat-completions">Get Started →</a>
<a class="btn-cta-ghost" href="/support">Contact Support</a>
</div>
</div>
</section>
</div>

## Quick Start

- Base URL: [https://api.acedata.cloud](https://api.acedata.cloud)
- Service page: [OpenAI generation on Ace Data Cloud](https://platform.acedata.cloud/service/openai)
- Docs: [Developer documentation](https://docs.acedata.cloud)

```bash
curl --request POST "https://api.acedata.cloud/openai/chat/completions" \
  --header "Authorization: Bearer YOUR_API_KEY" \
  --header "Content-Type: application/json" \
  --data '{}'
```

## APIs and Guides

Explore the supported endpoints and integration guides for OpenAI generation.

| API | Path | Integration Guidance |
| ---- | ---- | ------------ |
| [OpenAI Chat Completions API](https://platform.acedata.cloud/documents/1bcf3bba-102b-495d-9bba-47cd96717e45) | `/openai/chat/completions` | [OpenAI Chat Completion API Integration Guide](https://platform.acedata.cloud/documents/fc571e00-464f-429e-b920-8896c906c2b9) |
| [OpenAI Images Generations API](https://platform.acedata.cloud/documents/fd932485-90c7-45d6-8394-1e14b6f07b2b) | `/openai/images/generations` | [OpenAI Images Generations API Integration Guide](https://platform.acedata.cloud/documents/22fce352-b71e-4177-991f-2216841f35e2) |
| [OpenAI Responses API](https://platform.acedata.cloud/documents/81e285a6-d010-4a2d-a3a8-ca113d4ef82a) | `/openai/responses` | [OpenAI Responses API Integration Guide](https://platform.acedata.cloud/documents/c1da5338-9fff-4390-bbdc-29713893c07a) |
| [$t(document_title_openai_embeddings_api)](https://platform.acedata.cloud/documents/0f2e63fa-5890-4bdd-84f0-1706b5c9a387) | `/openai/embeddings` | [](https://platform.acedata.cloud/documents/) |
| [OpenAI Images Edits API](https://platform.acedata.cloud/documents/251f1efa-aaa6-462e-8af4-66854b1bc94d) | `/openai/images/edits` | [OpenAI Images Edits API Integration Guide](https://platform.acedata.cloud/documents/932e4b89-2cbb-4cb9-8f85-c9af256bfe69) |

## Related Resources

- [Ace Data Cloud Developer Platform](https://platform.acedata.cloud)
- [Ace Data Cloud Docs](https://docs.acedata.cloud)
- [Status Page](https://status.acedata.cloud)
- [Ace Data Cloud GitHub Organization](https://github.com/AceDataCloud)

## Support

If you meet any issue, please check [support info](https://platform.acedata.cloud/support) or browse the latest documentation on [docs.acedata.cloud](https://docs.acedata.cloud).