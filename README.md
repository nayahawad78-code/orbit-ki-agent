# orbit-ki-agent
Dunkle, moderne KI-Agent-Chat-Webseite mit Wikipedia-Suche und einfacher Rechenfunktion.
<!DOCTYPE html>
<html lang="de">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Orbit – KI-Agent</title>
  <style>
    :root {
      color-scheme: dark;
      --bg: #101114;
      --sidebar: #17181d;
      --panel: #1c1e24;
      --panel-hover: #252831;
      --text: #f2f2f5;
      --muted: #9a9ca8;
      --accent: #a68bfa;
      --border: #2b2d35;
    }

    * { box-sizing: border-box; }

    body {
      margin: 0;
      min-height: 100vh;
      background: var(--bg);
      color: var(--text);
      font-family: Inter, ui-sans-serif, system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
    }

    button, textarea { font: inherit; }

    .app {
      display: grid;
      grid-template-columns: 260px 1fr;
      min-height: 100vh;
    }

    aside {
      display: flex;
      flex-direction: column;
      gap: 24px;
      padding: 22px 16px;
      background: var(--sidebar);
      border-right: 1px solid var(--border);
    }

    .brand {
      display: flex;
      align-items: center;
      gap: 11px;
      padding: 4px 8px;
      font-size: 19px;
      font-weight: 700;
      letter-spacing: -.5px;
    }

    .brand-mark {
      display: grid;
      width: 34px;
      height: 34px;
      place-items: center;
      border-radius: 12px;
      background: linear-gradient(135deg, #a68bfa, #6d8bff);
      color: #14121d;
      font-size: 19px;
    }

    .new-chat {
      width: 100%;
      padding: 12px 14px;
      border: 1px solid var(--border);
      border-radius: 12px;
      background: var(--panel);
      color: var(--text);
      text-align: left;
      cursor: pointer;
    }

    .new-chat:hover, .suggestion:hover { background: var(--panel-hover); }

    .side-label {
      margin: 0 8px 8px;
      color: var(--muted);
      font-size: 12px;
      font-weight: 650;
      text-transform: uppercase;
      letter-spacing: .08em;
    }

    .agent-card {
      display: flex;
      gap: 11px;
      align-items: center;
      padding: 12px;
      border: 1px solid var(--border);
      border-radius: 13px;
      background: var(--panel);
    }

    .agent-icon {
      display: grid;
      width: 36px;
      height: 36px;
      flex: 0 0 auto;
      place-items: center;
      border-radius: 11px;
      background: #302748;
    }

    .agent-name { font-size: 14px; font-weight: 650; }
    .agent-status { margin-top: 4px; color: var(--muted); font-size: 12px; }

    .side-note {
      margin-top: auto;
      padding: 12px;
      color: var(--muted);
      font-size: 12px;
      line-height: 1.55;
    }

    main {
      display: flex;
      min-width: 0;
      height: 100vh;
      flex-direction: column;
    }

    header {
      display: flex;
      align-items: center;
      justify-content: space-between;
      padding: 18px 26px;
      border-bottom: 1px solid var(--border);
    }

    .header-title { font-size: 14px; font-weight: 650; }
    .header-tag {
      padding: 7px 10px;
      border: 1px solid var(--border);
      border-radius: 999px;
      color: var(--muted);
      font-size: 12px;
    }

    .conversation {
      width: min(850px, 100%);
      flex: 1;
      overflow-y: auto;
      margin: 0 auto;
      padding: 34px 26px 26px;
    }

    .welcome {
      padding: min(12vh, 100px) 0 28px;
      text-align: center;
    }

    .welcome-orb {
      display: grid;
      width: 58px;
      height: 58px;
      margin: 0 auto 20px;
      place-items: center;
      border: 1px solid #554579;
      border-radius: 20px;
      background: radial-gradient(circle at 30% 20%, #8e75d4, #392d57 70%);
      font-size: 26px;
      box-shadow: 0 0 45px #a68bfa20;
    }

    h1 { margin: 0; font-size: clamp(25px, 4vw, 34px); letter-spacing: -1px; }
    .welcome p { color: var(--muted); }

    .suggestions {
      display: grid;
      grid-template-columns: repeat(2, 1fr);
      gap: 10px;
      max-width: 620px;
      margin: 28px auto 0;
    }

    .suggestion {
      padding: 14px;
      border: 1px solid var(--border);
      border-radius: 13px;
      background: var(--panel);
      color: var(--text);
      text-align: left;
      cursor: pointer;
    }

    .suggestion span { display: block; margin-top: 5px; color: var(--muted); font-size: 12px; }

    .message {
      display: flex;
      gap: 12px;
      margin: 22px 0;
      line-height: 1.65;
      white-space: pre-wrap;
      overflow-wrap: anywhere;
    }

    .message.user { justify-content: flex-end; }

    .message-bubble {
      max-width: min(680px, 85%);
      padding: 13px 16px;
      border-radius: 16px;
      background: var(--panel);
    }

    .message.user .message-bubble {
      border: 1px solid #554579;
      background: #282239;
    }

    .avatar {
      display: grid;
      width: 30px;
      height: 30px;
      flex: 0 0 auto;
      place-items: center;
      border-radius: 10px;
      background: #302748;
    }

    .sources {
      display: flex;
      flex-wrap: wrap;
      gap: 8px;
      margin-top: 12px;
    }

    .sources a {
      padding: 6px 9px;
      border: 1px solid var(--border);
      border-radius: 999px;
      color: #c9b9ff;
      font-size: 12px;
      text-decoration: none;
    }

    .sources a:hover { background: var(--panel-hover); }

    .composer-wrap {
      padding: 14px 26px 22px;
      background: linear-gradient(transparent, var(--bg) 15%);
    }

    .composer {
      display: flex;
      align-items: flex-end;
      gap: 10px;
      width: min(800px, 100%);
      margin: 0 auto;
      padding: 10px;
      border: 1px solid #393b46;
      border-radius: 18px;
      background: var(--panel);
      box-shadow: 0 10px 40px #0003;
    }

    textarea {
      width: 100%;
      min-height: 28px;
      max-height: 150px;
      resize: none;
      border: 0;
      outline: 0;
      background: transparent;
      color: var(--text);
      padding: 7px 5px;
    }

    textarea::placeholder { color: #858793; }

    .send {
      width: 38px;
      height: 38px;
      flex: 0 0 auto;
      border: 0;
      border-radius: 12px;
      background: var(--accent);
      color: #171321;
      font-size: 18px;
      cursor: pointer;
    }

    .send:disabled { opacity: .5; cursor: wait; }

    .hint {
      margin: 9px auto 0;
      color: var(--muted);
      font-size: 11px;
      text-align: center;
    }

    @media (max-width: 700px) {
      .app { grid-template-columns: 1fr; }
      aside { display: none; }
      main { height: 100dvh; }
      header { padding: 15px 17px; }
      .conversation { padding: 20px 16px; }
      .composer-wrap { padding: 12px 14px 18px; }
      .suggestions { grid-template-columns: 1fr; }
      .welcome { padding-top: 8vh; }
    }
  </style>
</head>
<body>
  <div class="app">
    <aside>
      <div class="brand"><div class="brand-mark">✦</div> orbit</div>
      <button class="new-chat" id="newChat">＋ &nbsp; Neuer Chat</button>

      <section>
        <p class="side-label">Dein Agent</p>
        <div class="agent-card">
          <div class="agent-icon">✧</div>
          <div>
            <div class="agent-name">Wissens-Agent</div>
            <div class="agent-status">Wikipedia · Rechnen</div>
          </div>
        </div>
      </section>

      <div class="side-note">
        Der Agent kann Wikipedia durchsuchen und einfache Rechenaufgaben lösen.
        Für weitere KI-Funktionen kannst du eine eigene API verbinden.
      </div>
    </aside>

    <main>
      <header>
        <div class="header-title">Wissens-Agent</div>
        <div class="header-tag">● &nbsp; Bereit</div>
      </header>

      <section class="conversation" id="conversation" aria-live="polite">
        <div class="welcome" id="welcome">
          <div class="welcome-orb">✦</div>
          <h1>Was möchtest du wissen?</h1>
          <p>Frag nach einem Thema oder probier eine Aufgabe aus.</p>
          <div class="suggestions">
            <button class="suggestion" data-prompt="Was ist künstliche Intelligenz?">
              Was ist künstliche Intelligenz?<span>Eine kurze Erklärung mit Wikipedia-Quelle</span>
            </button>
            <button class="suggestion" data-prompt="Erkläre mir das Sonnensystem">
              Erkläre mir das Sonnensystem<span>Wissen kompakt zusammengefasst</span>
            </button>
            <button class="suggestion" data-prompt="Was ist 125 * 8?">
              Was ist 125 × 8?<span>Eine einfache Rechenaufgabe</span>
            </button>
            <button class="suggestion" data-prompt="Wer war Marie Curie?">
              Wer war Marie Curie?<span>Personen und Themen nachschlagen</span>
            </button>
          </div>
        </div>
      </section>

      <div class="composer-wrap">
        <form class="composer" id="chatForm">
          <textarea id="prompt" rows="1" placeholder="Schreib eine Nachricht …" aria-label="Nachricht"></textarea>
          <button class="send" id="sendButton" type="submit" aria-label="Senden">↑</button>
        </form>
        <div class="hint">Antworten ohne eigene API basieren auf Wikipedia oder einfacher Berechnung.</div>
      </div>
    </main>
  </div>

  <script>
    // Für echte KI-Antworten hier deinen eigenen Backend-Endpunkt eintragen.
    // Beispiel: const API_ENDPOINT = "/api/chat";
    const API_ENDPOINT = "";

    const conversation = document.getElementById("conversation");
    const welcome = document.getElementById("welcome");
    const form = document.getElementById("chatForm");
    const input = document.getElementById("prompt");
    const sendButton = document.getElementById("sendButton");
    const history = [];

    function addMessage(role, text, sources = []) {
      const row = document.createElement("div");
      row.className = `message ${role}`;

      if (role === "assistant") {
        const avatar = document.createElement("div");
        avatar.className = "avatar";
        avatar.textContent = "✧";
        row.appendChild(avatar);
      }

      const bubble = document.createElement("div");
      bubble.className = "message-bubble";
      bubble.textContent = text;

      if (sources.length) {
        const sourceList = document.createElement("div");
        sourceList.className = "sources";
        for (const source of sources) {
          const link = document.createElement("a");
          link.href = source.url;
          link.target = "_blank";
          link.rel = "noopener noreferrer";
          link.textContent = "Quelle: " + source.title;
          sourceList.appendChild(link);
        }
        bubble.appendChild(sourceList);
      }

      row.appendChild(bubble);
      conversation.appendChild(row);
      conversation.scrollTop = conversation.scrollHeight;
    }

    function parseCalculation(text) {
      const match = text.match(/[-+]?\d+(?:[.,]\d+)?(?:\s*[+\-*/]\s*[-+]?\d+(?:[.,]\d+)?)+/);
      if (!match) return null;

      const expression = match[0].replace(/,/g, ".");
      const tokens = expression.match(/\d+(?:\.\d+)?|[+\-*/()]/g);
      if (!tokens || tokens.join("") !== expression.replace(/\s/g, "")) return null;

      let index = 0;
      function factor() {
        const token = tokens[index++];
        if (token === "+") return factor();
        if (token === "-") return -factor();
        if (token === "(") {
          const value = expressionPart();
          if (tokens[index++] !== ")") throw new Error("Klammer fehlt");
          return value;
        }
        const value = Number(token);
        if (!Number.isFinite(value)) throw new Error("Ungültige Zahl");
        return value;
      }
      function term() {
        let value = factor();
        while (tokens[index] === "*" || tokens[index] === "/") {
          const op = tokens[index++];
          const next = factor();
          if (op === "/" && next === 0) throw new Error("Division durch null");
          value = op === "*" ? value * next : value / next;
        }
        return value;
      }
      function expressionPart() {
        let value = term();
        while (tokens[index] === "+" || tokens[index] === "-") {
          const op = tokens[index++];
          const next = term();
          value = op === "+" ? value + next : value - next;
        }
        return value;
      }

      try {
        const result = expressionPart();
        if (index !== tokens.length || !Number.isFinite(result)) return null;
        return `${expression} = ${result}`;
      } catch {
        return null;
      }
    }

    function makeWikiQuery(text) {
      return text
        .replace(/[?!.,]/g, " ")
        .replace(/^(was ist|wer ist|wer war|was war|erkläre mir|erklär mir|erkläre|erklär|suche nach|finde|such nach)\s+/i, "")
        .replace(/\s+/g, " ")
        .trim();
    }

    async function searchWikipedia(text) {
      const query = makeWikiQuery(text);
      const searchUrl = new URL("https://de.wikipedia.org/w/rest.php/v1/search/page");
      searchUrl.searchParams.set("q", query);
      searchUrl.searchParams.set("limit", "1");

      const searchResponse = await fetch(searchUrl);
      if (!searchResponse.ok) throw new Error("Wikipedia-Suche nicht erreichbar.");
      const searchData = await searchResponse.json();
      const page = searchData.pages?.[0];
      if (!page) return null;

      const summaryResponse = await fetch(
        `https://de.wikipedia.org/api/rest_v1/page/summary/${encodeURIComponent(page.key)}`
      );
      if (!summaryResponse.ok) throw new Error("Der Wikipedia-Artikel konnte nicht geladen werden.");
      const summary = await summaryResponse.json();

      return {
        title: summary.title || page.title,
        extract: summary.extract || page.description || "Zu diesem Thema wurde kein Kurztext gefunden.",
        url: summary.content_urls?.desktop?.page || `https://de.wikipedia.org/wiki/${encodeURIComponent(page.key)}`
      };
    }

    async function getReply(text) {
      const calculation = parseCalculation(text);

      if (!API_ENDPOINT && calculation) {
        return { text: calculation, sources: [] };
      }

      let wiki = null;
      try {
        wiki = await searchWikipedia(text);
      } catch (error) {
        if (!API_ENDPOINT) {
          return {
            text: "Ich konnte Wikipedia gerade nicht erreichen. Prüfe bitte deine Internetverbindung und versuch es noch einmal.",
            sources: []
          };
        }
      }

      if (API_ENDPOINT) {
        const response = await fetch(API_ENDPOINT, {
          method: "POST",
          headers: { "Content-Type": "application/json" },
          body: JSON.stringify({
            message: text,
            history,
            wikipediaContext: wiki ? { title: wiki.title, extract: wiki.extract, url: wiki.url } : null
          })
        });

        if (!response.ok) throw new Error("Deine KI-API hat einen Fehler zurückgegeben.");
        const data = await response.json();
        return {
          text: data.reply || data.message || "Die API hat keine Antwort zurückgegeben.",
          sources: wiki ? [{ title: wiki.title, url: wiki.url }] : []
        };
      }

      if (!wiki) {
        return {
          text: "Dazu habe ich keinen passenden Wikipedia-Artikel gefunden. Versuch es mit einem anderen Suchbegriff.",
          sources: []
        };
      }

      return {
        text: `${wiki.title}\n\n${wiki.extract}`,
        sources: [{ title: wiki.title, url: wiki.url }]
      };
    }

    async function sendMessage(text) {
      text = text.trim();
      if (!text || sendButton.disabled) return;

      welcome?.remove();
      addMessage("user", text);
      history.push({ role: "user", content: text });
      input.value = "";
      input.style.height = "auto";
      sendButton.disabled = true;

      const loading = document.createElement("div");
      loading.className = "message";
      loading.id = "loading";
      loading.innerHTML = '<div class="avatar">✧</div><div class="message-bubble">Ich suche …</div>';
      conversation.appendChild(loading);
      conversation.scrollTop = conversation.scrollHeight;

      try {
        const result = await getReply(text);
        loading.remove();
        addMessage("assistant", result.text, result.sources);
        history.push({ role: "assistant", content: result.text });
      } catch (error) {
        loading.remove();
        addMessage("assistant", error.message || "Es ist ein Fehler aufgetreten.");
      } finally {
        sendButton.disabled = false;
        input.focus();
      }
    }

    form.addEventListener("submit", (event) => {
      event.preventDefault();
      sendMessage(input.value);
    });

    input.addEventListener("input", () => {
      input.style.height = "auto";
      input.style.height = Math.min(input.scrollHeight, 150) + "px";
    });

    input.addEventListener("keydown", (event) => {
      if (event.key === "Enter" && !event.shiftKey) {
        event.preventDefault();
        form.requestSubmit();
      }
    });

    document.querySelectorAll(".suggestion").forEach((button) => {
      button.addEventListener("click", () => sendMessage(button.dataset.prompt));
    });

    document.getElementById("newChat").addEventListener("click", () => {
      history.length = 0;
      conversation.innerHTML = "";
      conversation.appendChild(welcome);
      input.focus();
    });
  </script>
</body>
</html>
