
<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>Juridique·IA — Rédaction juridique assistée par IA</title>
<meta name="description" content="Juridique·IA rédige pour vous lettres, contrats, conventions et attestations juridiques, en quelques minutes." />
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Fraunces:opsz,wght@9..144,400;9..144,500;9..144,600;9..144,700&family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
<script src="https://cdnjs.cloudflare.com/ajax/libs/react/18.2.0/umd/react.production.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/react-dom/18.2.0/umd/react-dom.production.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/babel-standalone/7.23.5/babel.min.js"></script>
<style>
  * { box-sizing: border-box; margin: 0; padding: 0; }
  html, body { width: 100%; min-height: 100%; }
  body { font-family: 'Inter', sans-serif; background: #FAFAF8; color: #1B2A4A; }
  #root { min-height: 100vh; }

  .jia-root {
    --ink: #1B2A4A;
    --ink-soft: #4A5D6B;
    --paper: #FAFAF8;
    --paper-alt: #F3F1EA;
    --line: #E3DFD3;
    --gold: #B8934A;
    --gold-soft: #EFE6D3;
    --white: #FFFFFF;
    --success: #3F6B4A;
    background: var(--paper);
    color: var(--ink);
    min-height: 100vh;
    width: 100%;
  }
  .serif { font-family: 'Fraunces', serif; }
  .seal-svg { color: var(--gold); }

  .nav {
    display: flex; align-items: center; justify-content: space-between;
    padding: 20px 40px; border-bottom: 1px solid var(--line);
    background: rgba(250,250,248,0.92); backdrop-filter: blur(8px);
    position: sticky; top: 0; z-index: 50;
  }
  .brand { display: flex; align-items: center; gap: 10px; cursor: pointer; }
  .brand-name { font-family: 'Fraunces', serif; font-weight: 600; font-size: 20px; letter-spacing: 0.3px; color: var(--ink); }
  .brand-name .dot { color: var(--gold); }
  .nav-links { display: flex; align-items: center; gap: 28px; }
  .nav-link { font-size: 14px; font-weight: 500; color: var(--ink-soft); cursor: pointer; transition: color .2s; background:none; border:none; font-family:'Inter',sans-serif; }
  .nav-link:hover { color: var(--ink); }
  .btn {
    font-family: 'Inter', sans-serif; font-weight: 600; font-size: 14px;
    padding: 11px 22px; border-radius: 3px; cursor: pointer; border: 1.5px solid transparent;
    display: inline-flex; align-items: center; gap: 8px; transition: all .2s;
  }
  .btn-primary { background: var(--ink); color: var(--paper); }
  .btn-primary:hover { background: #0F1C33; }
  .btn-ghost { background: transparent; color: var(--ink); border-color: var(--ink); }
  .btn-ghost:hover { background: var(--ink); color: var(--paper); }
  .btn-gold { background: var(--gold); color: var(--white); }
  .btn-gold:hover { background: #A17F3C; }
  .btn:disabled { opacity: 0.5; cursor: not-allowed; }

  .hero {
    display: flex; align-items: center; justify-content: space-between;
    padding: 90px 60px 100px; max-width: 1280px; margin: 0 auto; gap: 60px;
  }
  .hero-left { flex: 1.1; max-width: 620px; }
  .eyebrow {
    display: inline-flex; align-items: center; gap: 8px;
    font-size: 12px; font-weight: 600; letter-spacing: 1.5px; text-transform: uppercase;
    color: var(--gold); margin-bottom: 24px; padding: 6px 14px; border: 1px solid var(--gold-soft);
    border-radius: 20px; background: var(--gold-soft);
  }
  .hero h1 {
    font-family: 'Fraunces', serif; font-size: 56px; line-height: 1.08; font-weight: 600;
    margin: 0 0 24px; letter-spacing: -0.5px;
  }
  .hero h1 em { font-style: italic; color: var(--gold); }
  .hero p.lead { font-size: 18px; color: var(--ink-soft); line-height: 1.65; margin-bottom: 36px; max-width: 520px; }
  .hero-cta { display: flex; gap: 14px; margin-bottom: 40px; flex-wrap: wrap; }
  .hero-stats { display: flex; gap: 40px; padding-top: 28px; border-top: 1px solid var(--line); }
  .stat-num { font-family: 'Fraunces', serif; font-size: 28px; font-weight: 600; color: var(--ink); }
  .stat-label { font-size: 12.5px; color: var(--ink-soft); }

  .hero-right { flex: 1; display: flex; justify-content: center; align-items: center; position: relative; }
  .seal-hero { width: 340px; height: 340px; position: relative; }
  .seal-ring { position: absolute; inset: 0; border-radius: 50%; border: 1.5px dashed var(--gold-soft); animation: spin 40s linear infinite; }
  @keyframes spin { to { transform: rotate(360deg); } }
  .seal-core {
    position: absolute; inset: 30px; border-radius: 50%; background: var(--white);
    border: 2px solid var(--ink); display: flex; align-items: center; justify-content: center;
    box-shadow: 0 20px 60px -20px rgba(27,42,74,0.25);
  }
  .seal-core svg { width: 60%; height: 60%; color: var(--ink); }

  .section { max-width: 1280px; margin: 0 auto; padding: 80px 60px; }
  .section-alt { background: var(--paper-alt); }
  .section-head { max-width: 600px; margin-bottom: 48px; }
  .section-head .eyebrow { margin-bottom: 16px; }
  .section-head h2 { font-family: 'Fraunces', serif; font-size: 36px; font-weight: 600; margin: 0 0 14px; }
  .section-head p { color: var(--ink-soft); font-size: 16px; line-height: 1.6; }

  .grid-3 { display: grid; grid-template-columns: repeat(3, 1fr); gap: 24px; }
  .card {
    background: var(--white); border: 1px solid var(--line); border-radius: 4px;
    padding: 32px 28px; cursor: pointer; transition: all .25s;
  }
  .card:hover { border-color: var(--gold); transform: translateY(-3px); box-shadow: 0 16px 32px -20px rgba(27,42,74,0.2); }
  .card-icon { width: 42px; height: 42px; border-radius: 50%; background: var(--gold-soft); display: flex; align-items: center; justify-content: center; margin-bottom: 20px; color: var(--gold); }
  .card h3 { font-family: 'Fraunces', serif; font-size: 19px; font-weight: 600; margin: 0 0 8px; }
  .card p { font-size: 14px; color: var(--ink-soft); line-height: 1.5; margin: 0; }

  .trust-bar { display: flex; justify-content: center; gap: 60px; padding: 36px 0; border-top: 1px solid var(--line); border-bottom: 1px solid var(--line); flex-wrap: wrap; }
  .trust-item { display: flex; align-items: center; gap: 10px; font-size: 13px; color: var(--ink-soft); font-weight: 500; }
  .trust-item svg { color: var(--gold); }

  .auth-wrap { min-height: calc(100vh - 73px); display: flex; align-items: center; justify-content: center; padding: 60px 20px; }
  .auth-card { width: 100%; max-width: 420px; background: var(--white); border: 1px solid var(--line); border-radius: 6px; padding: 44px 40px; box-shadow: 0 30px 80px -40px rgba(27,42,74,0.25); }
  .auth-card .seal-svg { display: block; margin: 0 auto 20px; color: var(--ink); }
  .auth-card h2 { font-family: 'Fraunces', serif; text-align: center; font-size: 26px; margin: 0 0 6px; }
  .auth-card .sub { text-align: center; color: var(--ink-soft); font-size: 14px; margin-bottom: 32px; }
  .field { margin-bottom: 18px; }
  .field label { display: block; font-size: 13px; font-weight: 600; margin-bottom: 7px; color: var(--ink); }
  .input-wrap { position: relative; }
  .input-wrap svg { position: absolute; left: 13px; top: 50%; transform: translateY(-50%); color: var(--ink-soft); width: 16px; height: 16px; }
  .field input, .field textarea {
    width: 100%; padding: 12px 14px 12px 38px; border: 1.5px solid var(--line); border-radius: 3px;
    font-family: 'Inter', sans-serif; font-size: 14.5px; background: var(--paper); color: var(--ink);
    transition: border-color .2s;
  }
  .field textarea { padding-left: 14px; resize: vertical; min-height: 90px; }
  .field input:focus, .field textarea:focus { outline: none; border-color: var(--ink); background: var(--white); }
  .auth-error { background: #FBEAEA; color: #8C3B3B; font-size: 13px; padding: 10px 14px; border-radius: 3px; margin-bottom: 16px; }
  .auth-toggle { text-align: center; margin-top: 22px; font-size: 13.5px; color: var(--ink-soft); }
  .auth-toggle button { background: none; border: none; color: var(--ink); font-weight: 700; cursor: pointer; text-decoration: underline; font-family: 'Inter', sans-serif; }
  .auth-note { display: flex; gap: 8px; align-items: flex-start; background: var(--gold-soft); padding: 12px 14px; border-radius: 3px; font-size: 12px; color: #6B5730; margin-top: 20px; line-height: 1.5; }

  .dash { display: flex; min-height: calc(100vh - 73px); }
  .sidebar { width: 260px; border-right: 1px solid var(--line); padding: 32px 20px; flex-shrink: 0; }
  .side-user { display: flex; align-items: center; gap: 10px; padding: 10px 12px; background: var(--paper-alt); border-radius: 4px; margin-bottom: 28px; }
  .side-user .av { width: 34px; height: 34px; border-radius: 50%; background: var(--ink); color: var(--paper); display: flex; align-items: center; justify-content: center; font-weight: 700; font-size: 13px; font-family: 'Fraunces', serif; flex-shrink:0; }
  .side-user .name { font-size: 13.5px; font-weight: 600; overflow: hidden; text-overflow: ellipsis; white-space: nowrap; }
  .side-user .mail { font-size: 11.5px; color: var(--ink-soft); overflow: hidden; text-overflow: ellipsis; white-space: nowrap; }
  .side-link {
    display: flex; align-items: center; gap: 10px; width: 100%; padding: 11px 12px; border-radius: 4px;
    background: none; border: none; font-size: 14px; font-weight: 500; color: var(--ink-soft); cursor: pointer;
    margin-bottom: 4px; text-align: left; font-family: 'Inter', sans-serif;
  }
  .side-link:hover { background: var(--paper-alt); color: var(--ink); }
  .side-link.active { background: var(--ink); color: var(--paper); }
  .main { flex: 1; padding: 44px 48px; overflow-y: auto; }
  .main-head { display: flex; align-items: center; justify-content: space-between; margin-bottom: 32px; flex-wrap: wrap; gap: 16px; }
  .main-head h1 { font-family: 'Fraunces', serif; font-size: 28px; margin: 0; }
  .main-head p { color: var(--ink-soft); font-size: 14px; margin: 4px 0 0; }

  .doc-list { display: flex; flex-direction: column; gap: 10px; }
  .doc-row {
    display: flex; align-items: center; gap: 16px; padding: 16px 18px; background: var(--white);
    border: 1px solid var(--line); border-radius: 4px; transition: all .2s;
  }
  .doc-row:hover { border-color: var(--gold); }
  .doc-row .di { width: 38px; height: 38px; border-radius: 50%; background: var(--gold-soft); color: var(--gold); display: flex; align-items: center; justify-content: center; flex-shrink: 0; }
  .doc-row .dinfo { flex: 1; min-width: 0; }
  .doc-row .dinfo .t { font-weight: 600; font-size: 14.5px; overflow: hidden; text-overflow: ellipsis; white-space: nowrap; }
  .doc-row .dinfo .m { font-size: 12.5px; color: var(--ink-soft); }
  .icon-btn { background: none; border: none; cursor: pointer; color: var(--ink-soft); padding: 8px; border-radius: 4px; display:flex; }
  .icon-btn:hover { background: var(--paper-alt); color: var(--ink); }
  .empty-state { text-align: center; padding: 80px 20px; color: var(--ink-soft); }
  .empty-state svg { color: var(--line); margin-bottom: 16px; }

  .gen-wrap { max-width: 780px; }
  .type-grid { display: grid; grid-template-columns: repeat(2, 1fr); gap: 12px; margin-bottom: 32px; }
  .type-card {
    border: 1.5px solid var(--line); border-radius: 4px; padding: 16px 18px; cursor: pointer;
    background: var(--white); transition: all .2s;
  }
  .type-card:hover { border-color: var(--gold); }
  .type-card.selected { border-color: var(--ink); background: var(--paper-alt); }
  .type-card .tt { font-weight: 600; font-size: 14.5px; margin-bottom: 3px; }
  .type-card .td { font-size: 12.5px; color: var(--ink-soft); }
  .gen-form { background: var(--white); border: 1px solid var(--line); border-radius: 6px; padding: 32px; }
  .result-box { margin-top: 32px; background: var(--white); border: 1px solid var(--line); border-radius: 6px; overflow: hidden; }
  .result-head { display: flex; align-items: center; justify-content: space-between; padding: 18px 24px; border-bottom: 1px solid var(--line); background: var(--paper-alt); }
  .result-head .t { display: flex; align-items: center; gap: 10px; font-weight: 600; font-size: 14px; }
  .result-body { padding: 28px 32px; white-space: pre-wrap; font-size: 14.5px; line-height: 1.75; color: var(--ink); max-height: 500px; overflow-y: auto; font-family: 'Inter', sans-serif; }
  .disclaimer { display: flex; gap: 10px; padding: 14px 20px; background: var(--gold-soft); font-size: 12.5px; color: #6B5730; line-height: 1.5; align-items: flex-start; }

  .spinner { width: 16px; height: 16px; border: 2px solid rgba(255,255,255,0.3); border-top-color: currentColor; border-radius: 50%; animation: rot .7s linear infinite; }
  @keyframes rot { to { transform: rotate(360deg); } }

  footer { border-top: 1px solid var(--line); padding: 40px 60px; text-align: center; color: var(--ink-soft); font-size: 13px; }

  @media (max-width: 900px) {
    .nav { padding: 16px 20px; }
    .nav-links { gap: 14px; }
    .hero { flex-direction: column; padding: 50px 24px; }
    .hero h1 { font-size: 34px; }
    .grid-3 { grid-template-columns: 1fr; }
    .trust-bar { gap: 24px; }
    .dash { flex-direction: column; }
    .sidebar { width: 100%; border-right: none; border-bottom: 1px solid var(--line); }
    .main { padding: 28px 20px; }
    .type-grid { grid-template-columns: 1fr; }
    .section { padding: 50px 24px; }
    .seal-hero { width: 220px; height: 220px; }
  }
</style>
</head>
<body>
<div id="root"></div>

<script type="text/babel" data-presets="react">
const { useState, useRef } = React;

const DOC_TYPES = [
  { id: "lettre", label: "Lettre juridique", desc: "Mise en demeure, réclamation, courrier officiel" },
  { id: "contrat", label: "Contrat", desc: "Prestation, travail, location, vente" },
  { id: "convention", label: "Convention", desc: "Accord entre parties, partenariat" },
  { id: "resiliation", label: "Résiliation", desc: "Rupture de contrat, abonnement, bail" },
  { id: "attestation", label: "Attestation", desc: "Attestation sur l'honneur, témoignage" },
  { id: "autre", label: "Autre document", desc: "Décrivez librement votre besoin" },
];

// Stockage en mémoire (persiste tant que l'onglet reste ouvert).
// Pour une persistance réelle entre sessions, il faut un vrai backend (base de données).
let MEM = { users: [], docs: {} };

async function callClaude(prompt, system) {
  const res = await fetch("https://api.anthropic.com/v1/messages", {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify({
      model: "claude-sonnet-4-6",
      max_tokens: 1000,
      system,
      messages: [{ role: "user", content: prompt }],
    }),
  });
  const data = await res.json();
  return (data.content || []).map((b) => b.text || "").join("\n");
}

// --- Icônes SVG minimalistes (pas de dépendance externe) ---
const Icon = ({ path, size = 18, ...p }) => (
  <svg width={size} height={size} viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="1.8" strokeLinecap="round" strokeLinejoin="round" {...p}>{path}</svg>
);
const IFile = (p) => <Icon {...p} path={<><path d="M14 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V8z"/><path d="M14 2v6h6"/></>} />;
const IUser = (p) => <Icon {...p} path={<><circle cx="12" cy="8" r="4"/><path d="M4 21v-1a7 7 0 0 1 14 0v1"/></>} />;
const ILogOut = (p) => <Icon {...p} path={<><path d="M9 21H5a2 2 0 0 1-2-2V5a2 2 0 0 1 2-2h4"/><path d="M16 17l5-5-5-5"/><path d="M21 12H9"/></>} />;
const IPlus = (p) => <Icon {...p} path={<><path d="M12 5v14"/><path d="M5 12h14"/></>} />;
const IDownload = (p) => <Icon {...p} path={<><path d="M12 3v12"/><path d="m7 10 5 5 5-5"/><path d="M5 21h14"/></>} />;
const ITrash = (p) => <Icon {...p} path={<><path d="M3 6h18"/><path d="M19 6l-1 14a2 2 0 0 1-2 2H8a2 2 0 0 1-2-2L5 6"/><path d="M10 11v6"/><path d="M14 11v6"/><path d="M8 6V4a2 2 0 0 1 2-2h4a2 2 0 0 1 2 2v2"/></>} />;
const IChevronR = (p) => <Icon {...p} path={<path d="m9 18 6-6-6-6"/>} />;
const ILock = (p) => <Icon {...p} path={<><rect x="3" y="11" width="18" height="11" rx="2"/><path d="M7 11V7a5 5 0 0 1 10 0v4"/></>} />;
const IMail = (p) => <Icon {...p} path={<><rect x="2" y="4" width="20" height="16" rx="2"/><path d="m22 6-10 7L2 6"/></>} />;
const IKey = (p) => <Icon {...p} path={<><circle cx="7.5" cy="15.5" r="5.5"/><path d="m21 2-9.6 9.6"/><path d="m15.5 7.5 3 3L22 7l-3-3"/></>} />;
const IFolder = (p) => <Icon {...p} path={<path d="M4 4a2 2 0 0 0-2 2v12a2 2 0 0 0 2 2h16a2 2 0 0 0 2-2V8a2 2 0 0 0-2-2h-8l-2-2H4z"/>} />;
const ISparkles = (p) => <Icon {...p} path={<><path d="M12 3v4"/><path d="M12 17v4"/><path d="M3 12h4"/><path d="M17 12h4"/><path d="m6 6 2 2"/><path d="m16 16 2 2"/><path d="m6 18 2-2"/><path d="m16 8 2-2"/></>} />;
const IShield = (p) => <Icon {...p} path={<path d="M12 2 4 5v6c0 5.5 3.5 9 8 11 4.5-2 8-5.5 8-11V5z"/>} />;
const IScale = (p) => <Icon {...p} path={<><path d="M12 2v20"/><path d="m5 7 4 0"/><path d="m15 7 4 0"/><path d="M3 7l9-3 9 3"/><path d="M3 7l-2 6a3 3 0 0 0 6 0z"/><path d="M21 7l-2 6a3 3 0 0 0 6 0z"/></>} />;
const ISearch = (p) => <Icon {...p} path={<><circle cx="11" cy="11" r="8"/><path d="m21 21-4.3-4.3"/></>} />;
const IBook = (p) => <Icon {...p} path={<><path d="M4 19.5A2.5 2.5 0 0 1 6.5 17H20"/><path d="M6.5 2H20v20H6.5A2.5 2.5 0 0 1 4 19.5v-15A2.5 2.5 0 0 1 6.5 2z"/></>} />;
const IExternal = (p) => <Icon {...p} path={<><path d="M18 13v6a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2V8a2 2 0 0 1 2-2h6"/><path d="M15 3h6v6"/><path d="M10 14 21 3"/></>} />;

function Seal({ size = 40 }) {
  return (
    <svg width={size} height={size} viewBox="0 0 100 100" className="seal-svg">
      <circle cx="50" cy="50" r="46" fill="none" stroke="currentColor" strokeWidth="2.5" />
      <circle cx="50" cy="50" r="38" fill="none" stroke="currentColor" strokeWidth="1" strokeDasharray="2 3" />
      <path d="M50 28 L58 46 L50 42 L42 46 Z" fill="currentColor" />
      <path d="M35 55 Q50 48 65 55 L65 62 Q50 56 35 62 Z" fill="currentColor" />
      <text x="50" y="78" textAnchor="middle" fontSize="9" fill="currentColor" fontFamily="Fraunces, serif" letterSpacing="1">JURIDIQUE·IA</text>
    </svg>
  );
}

function App() {
  const [view, setView] = useState("home");
  const [authMode, setAuthMode] = useState("login");
  const [user, setUser] = useState(null);
  const [docs, setDocs] = useState([]);
  const [activeType, setActiveType] = useState(null);
  const [form, setForm] = useState({ context: "", nom: "", destinataire: "", details: "" });
  const [generating, setGenerating] = useState(false);
  const [result, setResult] = useState("");
  const [error, setError] = useState("");
  const resultRef = useRef(null);

  const [researchQuery, setResearchQuery] = useState("");
  const [researching, setResearching] = useState(false);
  const [researchResult, setResearchResult] = useState("");
  const [researchError, setResearchError] = useState("");

  const [authForm, setAuthForm] = useState({ email: "", password: "", name: "" });
  const [authError, setAuthError] = useState("");

  function handleAuth(e) {
    e.preventDefault();
    setAuthError("");
    if (!authForm.email || !authForm.password) { setAuthError("Merci de remplir tous les champs."); return; }
    if (authMode === "signup") {
      if (MEM.users.find((u) => u.email === authForm.email)) { setAuthError("Un compte existe déjà avec cet email."); return; }
      const newUser = { email: authForm.email, password: authForm.password, name: authForm.name || authForm.email.split("@")[0] };
      MEM.users.push(newUser);
      MEM.docs[newUser.email] = [];
      setUser(newUser);
      setDocs([]);
    } else {
      const found = MEM.users.find((u) => u.email === authForm.email && u.password === authForm.password);
      if (!found) { setAuthError("Email ou mot de passe incorrect."); return; }
      setUser(found);
      setDocs(MEM.docs[found.email] || []);
    }
    setView("dashboard");
    setAuthForm({ email: "", password: "", name: "" });
  }

  function logout() { setUser(null); setDocs([]); setView("home"); }

  function saveDoc(doc) {
    if (!user) return;
    const list = [doc, ...(MEM.docs[user.email] || [])];
    MEM.docs[user.email] = list;
    setDocs(list);
  }
  function deleteDoc(id) {
    if (!user) return;
    const list = (MEM.docs[user.email] || []).filter((d) => d.id !== id);
    MEM.docs[user.email] = list;
    setDocs(list);
  }

  async function handleGenerate(e) {
    e.preventDefault();
    setGenerating(true); setError(""); setResult("");
    try {
      const typeInfo = activeType ? DOC_TYPES.find((t) => t.id === activeType) : null;
      const system = "Tu es un assistant juridique francophone expert en rédaction de documents (lettres, contrats, conventions, résiliations, attestations, ou tout autre document juridique) conformes au droit français. Si le type de document n'est pas précisé, analyse la situation décrite par l'utilisateur et détermine toi-même le document le plus adapté (lettre de mise en demeure, contrat, résiliation, attestation, courrier officiel...), et indique ce choix en première ligne du document sous la forme [Type de document choisi : ...] avant de rédiger le contenu. Tu t'appuies sur les codes juridiques français en vigueur (Code civil, Code du travail, Code de la consommation, Code de commerce, Code de la construction et de l'habitation, Code monétaire et financier, etc.) selon le sujet du document. Quand c'est pertinent, tu cites précisément les articles de loi applicables (ex : conformément à l'article 1231-1 du Code civil, ou en application de l'article L. 224-79 du Code de la consommation). Tu rédiges des documents clairs, formels, complets et juridiquement structurés (mentions obligatoires, formules d'usage, dates, formule de politesse), en complétant intelligemment les informations manquantes par des mentions génériques ([Votre nom], [Adresse], etc.) plutôt que de bloquer la rédaction. Tu ne donnes jamais de conseil juridique définitif : tu précises en fin de document que ce texte est une aide à la rédaction générée par IA, que les références légales citées doivent être vérifiées sur Légifrance ou par un professionnel du droit avant tout usage, en particulier pour les cas complexes ou à fort enjeu. Réponds uniquement avec le document rédigé, prêt à être personnalisé, sans commentaire avant ou après.";
      const prompt = `Type de document : ${typeInfo ? typeInfo.label : "Non précisé — à toi de déterminer le document le plus adapté selon le contexte"}\nNom de l'expéditeur : ${form.nom || "[Non précisé]"}\nDestinataire : ${form.destinataire || "[Non précisé]"}\nContexte / situation : ${form.context}\nDétails complémentaires : ${form.details || "Aucun"}\n\nRédige ce document maintenant.`;
      const text = await callClaude(prompt, system);
      setResult(text);
      saveDoc({ id: Date.now().toString(), type: typeInfo ? typeInfo.label : "Déterminé par l'IA", title: `${typeInfo ? typeInfo.label : "Document"} — ${form.destinataire || "sans titre"}`, content: text, date: new Date().toLocaleDateString("fr-FR") });
      setTimeout(() => resultRef.current && resultRef.current.scrollIntoView({ behavior: "smooth" }), 100);
    } catch (err) {
      setError("Une erreur est survenue lors de la génération. Merci de réessayer.");
    } finally { setGenerating(false); }
  }

  async function handleResearch(e) {
    e.preventDefault();
    setResearching(true); setResearchError(""); setResearchResult("");
    try {
      const system = "Tu es un moteur de recherche juridique francophone. À partir d'une situation décrite par l'utilisateur, tu identifies les articles de loi français les plus pertinents en parcourant mentalement l'ensemble des codes juridiques français (Code civil, Code du travail, Code de la consommation, Code de commerce, Code pénal, Code de la construction et de l'habitation, Code monétaire et financier, Code de procédure civile, Code de la santé publique, Code de l'éducation, etc.). Pour chaque situation, réponds UNIQUEMENT avec une liste structurée au format suivant, sans texte d'introduction ni de conclusion :\n\n1. [Code] — Article [numéro]\nRésumé : [ce que dit l'article en une phrase, dans tes propres mots]\nPertinence : [pourquoi cet article s'applique à cette situation, en une phrase]\n\n(Répète pour chaque article pertinent, entre 2 et 6 articles selon la situation, du plus pertinent au moins pertinent.)\n\nÀ la toute fin, ajoute une ligne : Note : ces références doivent être vérifiées sur legifrance.gouv.fr, le droit évoluant régulièrement.";
      const prompt = `Situation à analyser : ${researchQuery}\n\nIdentifie les articles de loi français applicables.`;
      const text = await callClaude(prompt, system);
      setResearchResult(text);
    } catch (err) {
      setResearchError("Une erreur est survenue lors de la recherche. Merci de réessayer.");
    } finally { setResearching(false); }
  }

  function useResearchForDoc() {
    setForm({ ...form, context: researchQuery });
    setActiveType(null);
    setResult("");
    setView("generate");
  }

  function downloadDoc(doc) {
    const blob = new Blob([doc.content], { type: "text/plain;charset=utf-8" });
    const url = URL.createObjectURL(blob);
    const a = document.createElement("a");
    a.href = url; a.download = `${doc.title.replace(/[^a-z0-9]/gi, "_")}.txt`; a.click();
    URL.revokeObjectURL(url);
  }

  return (
    <div className="jia-root">
      <nav className="nav">
        <div className="brand" onClick={() => setView(user ? "dashboard" : "home")}>
          <Seal size={30} />
          <span className="brand-name">Juridique<span className="dot">·</span>IA</span>
        </div>
        {!user ? (
          <div className="nav-links">
            <button className="nav-link" onClick={() => setView("home")}>Accueil</button>
            <button className="nav-link" onClick={() => { setAuthMode("login"); setView("auth"); }}>Connexion</button>
            <button className="btn btn-primary" onClick={() => { setAuthMode("signup"); setView("auth"); }}>Créer un compte</button>
          </div>
        ) : (
          <div className="nav-links">
            <button className="nav-link" onClick={() => setView("dashboard")}>Tableau de bord</button>
            <button className="btn btn-ghost" onClick={logout}><ILogOut size={15} /> Déconnexion</button>
          </div>
        )}
      </nav>

      {view === "home" && (
        <React.Fragment>
          <div className="hero">
            <div className="hero-left">
              <div className="eyebrow"><IShield size={13} /> Rédaction juridique assistée par IA</div>
              <h1>Vos documents juridiques, <em>rédigés</em> avec justesse.</h1>
              <p className="lead">Lettres, contrats, conventions, résiliations : Juridique·IA rédige pour vous des documents clairs, structurés et conformes, en quelques minutes. Sécurisé, personnalisable, à votre image.</p>
              <div className="hero-cta">
                <button className="btn btn-primary" onClick={() => { setAuthMode("signup"); setView("auth"); }}>Créer mon compte <IChevronR size={16} /></button>
                <button className="btn btn-ghost" onClick={() => { setAuthMode("login"); setView("auth"); }}>J'ai déjà un compte</button>
              </div>
              <div className="hero-stats">
                <div><div className="stat-num">6</div><div className="stat-label">Types de documents</div></div>
                <div><div className="stat-num">100%</div><div className="stat-label">Personnalisable</div></div>
                <div><div className="stat-num">&lt;2min</div><div className="stat-label">Temps de rédaction</div></div>
              </div>
            </div>
            <div className="hero-right">
              <div className="seal-hero">
                <div className="seal-ring" />
                <div className="seal-core"><Seal size={200} /></div>
              </div>
            </div>
          </div>

          <div className="trust-bar">
            <div className="trust-item"><ILock size={16} /> Données chiffrées</div>
            <div className="trust-item"><IShield size={16} /> Compte sécurisé</div>
            <div className="trust-item"><IFile size={16} /> Documents éditables</div>
            <div className="trust-item"><IScale size={16} /> Articles de loi cités</div>
          </div>

          <div className="section">
            <div className="section-head">
              <div className="eyebrow">Nos services</div>
              <h2>Un document pour chaque situation</h2>
              <p>Sélectionnez le type de document dont vous avez besoin, décrivez votre situation, et laissez Juridique·IA rédiger un premier jet complet et personnalisable.</p>
            </div>
            <div className="grid-3">
              {DOC_TYPES.map((t) => (
                <div key={t.id} className="card" onClick={() => { setActiveType(t.id); if (user) { setView("generate"); } else { setAuthMode("signup"); setView("auth"); } }}>
                  <div className="card-icon"><IFile size={20} /></div>
                  <h3>{t.label}</h3>
                  <p>{t.desc}</p>
                </div>
              ))}
            </div>
          </div>

          <div className="section">
            <div className="section-head">
              <div className="eyebrow">Nouveau</div>
              <h2>Recherche juridique</h2>
              <p>Vous ne savez pas quels articles de loi s'appliquent à votre situation ? Décrivez-la, l'IA parcourt les codes juridiques français et vous transmet les articles pertinents, avec un résumé et le lien vers Légifrance pour vérification.</p>
            </div>
            <div className="card" style={{maxWidth: 420, display:"flex", gap:20, alignItems:"flex-start"}} onClick={() => { if (user) { setResearchResult(""); setView("research"); } else { setAuthMode("signup"); setView("auth"); } }}>
              <div className="card-icon" style={{flexShrink:0}}><ISearch size={20} /></div>
              <div>
                <h3>Trouver les bons articles</h3>
                <p>Décrivez votre situation en langage courant et obtenez les références légales applicables, prêtes à être vérifiées ou intégrées à un document.</p>
              </div>
            </div>
          </div>

          <div className="section section-alt">
            <div className="section-head">
              <div className="eyebrow">Comment ça marche</div>
              <h2>Trois étapes, un document prêt</h2>
            </div>
            <div className="grid-3">
              <div className="card" style={{cursor:"default"}}>
                <div className="card-icon"><IUser size={20} /></div>
                <h3>1. Créez votre compte</h3>
                <p>Un espace personnel sécurisé pour retrouver tous vos documents à tout moment.</p>
              </div>
              <div className="card" style={{cursor:"default"}}>
                <div className="card-icon"><ISparkles size={20} /></div>
                <h3>2. Décrivez votre besoin</h3>
                <p>Choisissez un type de document et expliquez votre situation en quelques phrases.</p>
              </div>
              <div className="card" style={{cursor:"default"}}>
                <div className="card-icon"><IDownload size={20} /></div>
                <h3>3. Téléchargez et signez</h3>
                <p>Récupérez votre document rédigé, personnalisez-le et téléchargez-le.</p>
              </div>
            </div>
          </div>

          <footer>© 2026 Juridique·IA — Cet outil aide à la rédaction de documents mais ne remplace pas l'avis d'un professionnel du droit pour les situations complexes.</footer>
        </React.Fragment>
      )}

      {view === "auth" && (
        <div className="auth-wrap">
          <div className="auth-card">
            <Seal size={44} />
            <h2>{authMode === "login" ? "Connexion" : "Créer un compte"}</h2>
            <div className="sub">{authMode === "login" ? "Accédez à votre espace Juridique·IA" : "Rejoignez Juridique·IA en quelques secondes"}</div>
            {authError && <div className="auth-error">{authError}</div>}
            <form onSubmit={handleAuth}>
              {authMode === "signup" && (
                <div className="field">
                  <label>Nom complet</label>
                  <div className="input-wrap">
                    <IUser size={16} />
                    <input value={authForm.name} onChange={(e) => setAuthForm({ ...authForm, name: e.target.value })} placeholder="Jean Dupont" />
                  </div>
                </div>
              )}
              <div className="field">
                <label>Adresse email</label>
                <div className="input-wrap">
                  <IMail size={16} />
                  <input type="email" value={authForm.email} onChange={(e) => setAuthForm({ ...authForm, email: e.target.value })} placeholder="vous@exemple.fr" required />
                </div>
              </div>
              <div className="field">
                <label>Mot de passe</label>
                <div className="input-wrap">
                  <IKey size={16} />
                  <input type="password" value={authForm.password} onChange={(e) => setAuthForm({ ...authForm, password: e.target.value })} placeholder="••••••••" required />
                </div>
              </div>
              <button type="submit" className="btn btn-primary" style={{ width: "100%", justifyContent: "center", marginTop: 6 }}>
                {authMode === "login" ? "Se connecter" : "Créer mon compte"}
              </button>
            </form>
            <div className="auth-toggle">
              {authMode === "login" ? (
                <React.Fragment>Pas encore de compte ? <button onClick={() => { setAuthMode("signup"); setAuthError(""); }}>S'inscrire</button></React.Fragment>
              ) : (
                <React.Fragment>Déjà un compte ? <button onClick={() => { setAuthMode("login"); setAuthError(""); }}>Se connecter</button></React.Fragment>
              )}
            </div>
            <div className="auth-note">
              <ILock size={14} style={{flexShrink:0, marginTop:1}} />
              <span>Démo : ce prototype simule l'authentification en mémoire (dans votre navigateur, tant que l'onglet reste ouvert). Un déploiement réel nécessite un vrai backend sécurisé (chiffrement, base de données, HTTPS).</span>
            </div>
          </div>
        </div>
      )}

      {view === "dashboard" && user && (
        <div className="dash">
          <div className="sidebar">
            <div className="side-user">
              <div className="av">{user.name.slice(0,2).toUpperCase()}</div>
              <div style={{minWidth:0}}>
                <div className="name">{user.name}</div>
                <div className="mail">{user.email}</div>
              </div>
            </div>
            <button className="side-link active"><IFolder size={16} /> Mes documents</button>
            <button className="side-link" onClick={() => { setActiveType(null); setResult(""); setView("generate"); }}><IPlus size={16} /> Nouveau document</button>
            <button className="side-link" onClick={() => { setResearchResult(""); setView("research"); }}><ISearch size={16} /> Recherche juridique</button>
            <button className="side-link" onClick={logout}><ILogOut size={16} /> Déconnexion</button>
          </div>
          <div className="main">
            <div className="main-head">
              <div>
                <h1>Mes documents</h1>
                <p>{docs.length} document{docs.length !== 1 ? "s" : ""} enregistré{docs.length !== 1 ? "s" : ""}</p>
              </div>
              <div style={{display:"flex", gap:10}}>
                <button className="btn btn-ghost" onClick={() => { setResearchResult(""); setView("research"); }}><ISearch size={16} /> Recherche juridique</button>
                <button className="btn btn-primary" onClick={() => { setActiveType(null); setResult(""); setView("generate"); }}><IPlus size={16} /> Nouveau document</button>
              </div>
            </div>
            {docs.length === 0 ? (
              <div className="empty-state">
                <IFile size={44} style={{margin: "0 auto"}} />
                <p style={{fontSize:15, fontWeight:600, color:"var(--ink)", marginBottom:6}}>Aucun document pour le moment</p>
                <p>Créez votre premier document juridique en quelques clics.</p>
              </div>
            ) : (
              <div className="doc-list">
                {docs.map((d) => (
                  <div className="doc-row" key={d.id}>
                    <div className="di"><IFile size={17} /></div>
                    <div className="dinfo">
                      <div className="t">{d.title}</div>
                      <div className="m">{d.type} · {d.date}</div>
                    </div>
                    <button className="icon-btn" onClick={() => { setResult(d.content); setActiveType(null); setView("doc"); }}><IChevronR size={17} /></button>
                    <button className="icon-btn" onClick={() => downloadDoc(d)}><IDownload size={16} /></button>
                    <button className="icon-btn" onClick={() => deleteDoc(d.id)}><ITrash size={16} /></button>
                  </div>
                ))}
              </div>
            )}
          </div>
        </div>
      )}

      {view === "generate" && user && (
        <div className="dash">
          <div className="sidebar">
            <div className="side-user">
              <div className="av">{user.name.slice(0,2).toUpperCase()}</div>
              <div style={{minWidth:0}}>
                <div className="name">{user.name}</div>
                <div className="mail">{user.email}</div>
              </div>
            </div>
            <button className="side-link" onClick={() => setView("dashboard")}><IFolder size={16} /> Mes documents</button>
            <button className="side-link active"><IPlus size={16} /> Nouveau document</button>
            <button className="side-link" onClick={() => { setResearchResult(""); setView("research"); }}><ISearch size={16} /> Recherche juridique</button>
            <button className="side-link" onClick={logout}><ILogOut size={16} /> Déconnexion</button>
          </div>
          <div className="main">
            <div className="gen-wrap">
              <div className="main-head">
                <div>
                  <h1>Nouveau document</h1>
                  <p>Décrivez votre besoin, l'IA choisit le document adapté et le rédige</p>
                </div>
              </div>

              <div className="gen-form" style={{marginBottom: 24}}>
                <div className="field">
                  <label>Expliquez votre situation *</label>
                  <textarea required value={form.context} onChange={(e) => setForm({ ...form, context: e.target.value })} placeholder="Ex : Mon propriétaire ne me rend pas ma caution depuis 3 mois alors que j'ai quitté le logement en bon état le 1er mars. Je veux le mettre en demeure de me la rendre." style={{minHeight: 130}} />
                </div>
                <button type="button" className="btn btn-primary" disabled={generating || !form.context.trim()} style={{width:"100%", justifyContent:"center"}} onClick={handleGenerate}>
                  {generating ? (<React.Fragment><span className="spinner" /> L'IA rédige votre document...</React.Fragment>) : (<React.Fragment><ISparkles size={16} /> Laisser l'IA rédiger le document</React.Fragment>)}
                </button>
                {error && <div className="auth-error" style={{marginTop:16}}>{error}</div>}
              </div>

              <details style={{marginBottom: 8}}>
                <summary style={{cursor:"pointer", fontSize:13.5, fontWeight:600, color:"var(--ink-soft)", marginBottom: 16}}>Préciser le type de document et les destinataires (optionnel)</summary>
                <div className="type-grid" style={{marginTop: 16}}>
                  {DOC_TYPES.map((t) => (
                    <div key={t.id} className={"type-card" + (activeType === t.id ? " selected" : "")} onClick={() => setActiveType(activeType === t.id ? null : t.id)}>
                      <div className="tt">{t.label}</div>
                      <div className="td">{t.desc}</div>
                    </div>
                  ))}
                </div>
                <div className="gen-form" style={{marginTop: 16}}>
                  <div className="field">
                    <label>Votre nom (expéditeur)</label>
                    <input value={form.nom} onChange={(e) => setForm({ ...form, nom: e.target.value })} placeholder="Jean Dupont" />
                  </div>
                  <div className="field" style={{marginBottom: 0}}>
                    <label>Destinataire</label>
                    <input value={form.destinataire} onChange={(e) => setForm({ ...form, destinataire: e.target.value })} placeholder="Société XYZ, propriétaire, etc." />
                  </div>
                  <div className="field" style={{marginTop: 18, marginBottom: 0}}>
                    <label>Détails complémentaires</label>
                    <textarea value={form.details} onChange={(e) => setForm({ ...form, details: e.target.value })} placeholder="Références de contrat, montants, délais, etc." />
                  </div>
                </div>
              </details>

              {result && (
                <div className="result-box" ref={resultRef}>
                  <div className="result-head">
                    <div className="t"><IFile size={16} /> Document généré</div>
                    <div style={{display:"flex", gap:6}}>
                      <button className="icon-btn" onClick={() => downloadDoc({ title: (DOC_TYPES.find(t=>t.id===activeType) || {}).label || "document", content: result })}><IDownload size={16} /></button>
                    </div>
                  </div>
                  <div className="result-body">{result}</div>
                  <div className="disclaimer">
                    <IScale size={14} style={{flexShrink:0, marginTop:1}} />
                    <span>Ce document est une aide à la rédaction générée par IA, qui s'appuie sur les codes juridiques français. Vérifiez les articles de loi cités sur legifrance.gouv.fr avant tout usage, et faites relire le document par un professionnel du droit pour toute situation complexe ou à fort enjeu.</span>
                  </div>
                </div>
              )}
            </div>
          </div>
        </div>
      )}

      {view === "research" && user && (
        <div className="dash">
          <div className="sidebar">
            <div className="side-user">
              <div className="av">{user.name.slice(0,2).toUpperCase()}</div>
              <div style={{minWidth:0}}>
                <div className="name">{user.name}</div>
                <div className="mail">{user.email}</div>
              </div>
            </div>
            <button className="side-link" onClick={() => setView("dashboard")}><IFolder size={16} /> Mes documents</button>
            <button className="side-link" onClick={() => { setActiveType(null); setResult(""); setView("generate"); }}><IPlus size={16} /> Nouveau document</button>
            <button className="side-link active"><ISearch size={16} /> Recherche juridique</button>
            <button className="side-link" onClick={logout}><ILogOut size={16} /> Déconnexion</button>
          </div>
          <div className="main">
            <div className="gen-wrap">
              <div className="main-head">
                <div>
                  <h1>Recherche juridique</h1>
                  <p>Décrivez une situation, l'IA identifie les articles de loi applicables</p>
                </div>
              </div>

              <form className="gen-form" onSubmit={handleResearch}>
                <div className="field" style={{marginBottom: 18}}>
                  <label>Décrivez la situation *</label>
                  <textarea required value={researchQuery} onChange={(e) => setResearchQuery(e.target.value)} placeholder="Ex : Mon employeur me demande de travailler le dimanche sans compensation, est-ce légal ?" style={{minHeight: 110}} />
                </div>
                <button type="submit" className="btn btn-primary" disabled={researching || !researchQuery.trim()} style={{width:"100%", justifyContent:"center"}}>
                  {researching ? (<React.Fragment><span className="spinner" /> Recherche des articles applicables...</React.Fragment>) : (<React.Fragment><ISearch size={16} /> Rechercher les articles de loi</React.Fragment>)}
                </button>
                {researchError && <div className="auth-error" style={{marginTop:16}}>{researchError}</div>}
              </form>

              {researchResult && (
                <div className="result-box">
                  <div className="result-head">
                    <div className="t"><IBook size={16} /> Articles de loi identifiés</div>
                    <button className="btn btn-gold" style={{padding:"7px 14px", fontSize:13}} onClick={useResearchForDoc}><ISparkles size={14} /> Rédiger un document à partir de cette recherche</button>
                  </div>
                  <div className="result-body">{researchResult}</div>
                  <div className="disclaimer">
                    <IExternal size={14} style={{flexShrink:0, marginTop:1}} />
                    <span>Vérifiez le texte exact et à jour de chaque article sur <a href="https://www.legifrance.gouv.fr" target="_blank" rel="noopener noreferrer" style={{color:"#6B5730", fontWeight:600}}>legifrance.gouv.fr</a>. Cette recherche est une aide générée par IA et ne remplace pas la consultation d'un professionnel du droit.</span>
                  </div>
                </div>
              )}
            </div>
          </div>
        </div>
      )}

      {view === "doc" && user && (
        <div className="dash">
          <div className="sidebar">
            <div className="side-user">
              <div className="av">{user.name.slice(0,2).toUpperCase()}</div>
              <div style={{minWidth:0}}>
                <div className="name">{user.name}</div>
                <div className="mail">{user.email}</div>
              </div>
            </div>
            <button className="side-link" onClick={() => setView("dashboard")}><IFolder size={16} /> Mes documents</button>
            <button className="side-link" onClick={() => { setActiveType(null); setResult(""); setView("generate"); }}><IPlus size={16} /> Nouveau document</button>
            <button className="side-link" onClick={logout}><ILogOut size={16} /> Déconnexion</button>
          </div>
          <div className="main">
            <div className="gen-wrap">
              <button className="nav-link" style={{marginBottom:20}} onClick={() => setView("dashboard")}>← Retour aux documents</button>
              <div className="result-box">
                <div className="result-head"><div className="t"><IFile size={16} /> Document</div></div>
                <div className="result-body">{result}</div>
              </div>
            </div>
          </div>
        </div>
      )}
    </div>
  );
}

ReactDOM.createRoot(document.getElementById("root")).render(<App />);
</script>
</body>
</html>
