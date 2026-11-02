Act as a Principal Full-Stack Engineer and UX Designer. Build "HunLab", a modern cybersecurity learning and practice platform inspired by clean tech platforms like GitHub, Google, and Vercel. 

### UI/UX Rules & Aesthetic Guidelines:
1. Color Palette: 
   - Primary Accent: Light Blue (`#38BDF8` / `sky-400`)
   - Secondary Accent: Electric Lime Green (`#A3E635` / `lime-400`)
   - Neutral Light Mode: Background `#FAFAFA`, Cards `#FFFFFF`, Text `#0F172A`
   - Neutral Dark Mode: Background `#090D16`, Cards `#111827`, Text `#F8FAFC`
   - NO tacky neon cyber-glows or cheap Matrix-style hacker graphics. Use clean borders (`border-slate-200` / `border-slate-800`), crisp typography (Inter/Geist), subtle rounded corners (`rounded-xl`), and micro-interactions.
2. Global Layout Structure:
   - Top Bar: Logo (HunLab in clean font with a subtle lime dot), Search bar (`Ctrl+K` shortcut modal), Language Selector (English, Arabic RTL support, Russian), Theme Toggle, "Donate / Support Us" button, Login / Sign Up buttons, and User Profile menu.
   - Sub-Header Navigation: Roadmaps, Modules, SOC Practice Tests, AI Security Tutor, Tier Upgrade.
3. Banner & Cookie Notice:
   - Sticky bottom cookie banner: "We value your privacy. We use cookies to enhance your browsing experience, serve personalized ads or content, and analyze our traffic." 
   - Buttons: "Customize", "Reject All", "Accept All".

### Core Features & Data Architecture:
1. User Roles & Subscriptions:
   - User Tiers: Free, VIP, Admin, Mega, Prince.
   - Restrict advanced rooms, downloadable lab VMs, and automated AI Tutor feedback based on Tier level.
   - Include a clear, clean "Support Us / Pricing Tier" comparison table.
2. Learning Paths & Roadmap Viewer:
   - Interactive step-by-step learning nodes inspired by roadmap.sh (Linux Basics -> Bash Scripting -> Python & Tkinter GUI -> Networking Fundamentals -> Red Team / Blue Team Core -> Advanced CSA Certification).
   - Each node contains deep text articles, code snippets, syntax breakdowns, and links to relevant tools.
3. Practice Test Engine (CSA 100-Question System):
   - Multi-choice exam UI with immediate flag/answer verification, explanation popups, score tracking, and progress persistence.
   - Add a full dataset of Certified SOC Analyst questions (e.g., Q1: UrlScan for SQLi filtering; Q2: Zero-Day Attack; Q3: Collect, Ingest, Validate, Document, Report, Respond; etc.).

Create a dedicated component `SOCAnalystExam.tsx` for HunLab using React and TypeScript. 

Requirements:
1. Include an interactive quiz state where users can answer 100 questions sequentially or jump via a question grid index.
2. Render choices cleanly as selectable list items with subtle hover states.
3. Show progress indicators (e.g., "Question 4 of 100", time elapsed, current score).
4. Integrate an instant check mode that reveals the correct answer with detailed SOC workflow explanations upon submission.

Seed the question array with the provided question set (Questions 1 to 100):
- Q1 Answer: B (UrlScan)
- Q2 Answer: C (Zero-Day Attack)
- Q3 Answer: A (Collect, Ingest, Validate, Document, Report, Respond)
- Q4 Answer: D (She should formally raise a ticket and forward it to the IRT)
- Q5 Answer: A (Cross-site Scripting Attack)
- Q6 Answer: C (Parameter Tampering Attack)
- Q7 Answer: C (XSS Attack)
- Q8 Answer: A (Directory Traversal Attack)
- Q9 Answer: A (Tactics, Techniques, and Procedures)
- Q10 Answer: D (Reconnaissance Attack)
...and ensure all 100 questions are fully parsed into a JSON array with `id`, `question`, `options`, `correctAnswer`, and `explanation`.

Create a clean Pricing/Subscription section `SubscriptionTiers.tsx` for HunLab with 5 tiers:
1. Free ($0/mo): Access to basic Linux, Bash, and Python roadmaps + 10 free CSA practice questions. Includes standard community support.
2. VIP ($9.99/mo): Full access to all 100+ SOC practice questions, full Red/Blue team paths, and ad-free browsing.
3. Admin ($24.99/mo): VIP features + custom lab creation dashboard, user progress tracking, and downloadable lab notes.
4. Mega ($49.99/mo): Admin features + 1-on-1 AI Tutor assistance, priority lab server allocation, and automated certificate generation.
5. Prince ($99.99/mo / Lifetime option): Ultimate tier with unrestricted access, custom profile badges, direct support line, and early access to upcoming AI/DevOps modules.

Add a prominent top "Support Us (Free Tier)" button that opens a simple donation modal (BuyMeACoffee / Crypto / Stripe One-Time tip).

Integrating **xterm.js** into a Next.js (App Router or Pages Router) application requires client-side dynamic rendering because xterm.js relies directly on browser DOM APIs (`window` and `document`).

Here is how to set up an interactive terminal component with Next.js, TypeScript, and Tailwind CSS.

---

### Step 1: Install Dependencies

Run the following command in your Next.js project root:

```bash
npm install @xterm/xterm @xterm/addon-fit

```

---

### Step 2: Create the Client-Side Terminal Component

Create a client-side component (e.g., `components/Terminal.tsx`). Using `'use client'` is mandatory to prevent Next.js from attempting server-side rendering (SSR) on xterm.js.

```tsx
'use client';

import { useEffect, useRef } from 'react';
import { Terminal } from '@xterm/xterm';
import { FitAddon } from '@xterm/addon-fit';
import '@xterm/xterm/css/xterm.css';

interface TerminalProps {
  onCommand?: (command: string) => void;
}

export default function WebTerminal({ onCommand }: TerminalProps) {
  const terminalRef = useRef<HTMLDivElement | null>(null);
  const xtermRef = useRef<Terminal | null>(null);

  useEffect(() => {
    if (!terminalRef.current) return;

    // 1. Initialize Terminal instance with custom palette matching your UI
    const term = new Terminal({
      cursorBlink: true,
      fontFamily: 'Menlo, Monaco, "Courier New", monospace',
      fontSize: 14,
      theme: {
        background: '#090D16', // Deep dark slate
        foreground: '#F8FAFC',
        cursor: '#A3E635',     // Lime green cursor
        selectionBackground: '#38BDF844',
      },
    });

    const fitAddon = new FitAddon();
    term.loadAddon(fitAddon);

    // 2. Attach terminal to DOM container
    term.open(terminalRef.current);
    fitAddon.fit();

    xtermRef.current = term;

    // 3. Command Buffer & Prompt Logic
    let currentInput = '';
    const prompt = () => term.write('\r\n\x1b[38;2;56;189;248muser@hunlab\x1b[0m:\x1b[38;2;163;230;53m~\x1b[0m$ ');

    term.writeln('Welcome to HunLab Web Terminal v1.0.0');
    term.writeln('Type "help" or run Linux commands to start.');
    prompt();

    // 4. Handle Key Inputs
    const disposable = term.onData((data) => {
      const code = data.charCodeAt(0);

      if (code === 13) { // Enter key
        const command = currentInput.trim();
        term.write('\r\n');
        
        if (command) {
          handleLocalCommand(command, term);
          if (onCommand) onCommand(command);
        }
        
        currentInput = '';
        prompt();
      } else if (code === 127) { // Backspace key
        if (currentInput.length > 0) {
          currentInput = currentInput.slice(0, -1);
          term.write('\b \b');
        }
      } else if (code >= 32) { // Printable characters
        currentInput += data;
        term.write(data);
      }
    });

    // 5. Handle Window Resize
    const handleResize = () => fitAddon.fit();
    window.addEventListener('resize', handleResize);

    return () => {
      disposable.dispose();
      window.removeEventListener('resize', handleResize);
      term.dispose();
    };
  }, [onCommand]);

  // Simple client-side mock execution
  const handleLocalCommand = (cmd: string, term: Terminal) => {
    switch (cmd.toLowerCase()) {
      case 'help':
        term.writeln('Available commands: help, clear, whoami, uname -a');
        break;
      case 'clear':
        term.clear();
        break;
      case 'whoami':
        term.writeln('hunlab_student');
        break;
      case 'uname -a':
        term.writeln('Linux hunlab-node 6.1.0-generic #1 SMP x86_64 GNU/Linux');
        break;
      default:
        term.writeln(`bash: ${cmd}: command not found`);
        break;
    }
  };

  return (
    <div className="w-full h-96 p-3 bg-[#090D16] border border-slate-800 rounded-xl overflow-hidden shadow-2xl">
      <div ref={terminalRef} className="w-full h-full" />
    </div>
  );
}

```

*Verification:* When the page loads in a browser, you will see a stylized dark terminal window with a flashing lime cursor accepting typed keyboard inputs.

---

### Step 3: Dynamically Import in Next.js Pages / App Routes

To bypass SSR errors completely when embedding the terminal inside your pages or dashboard, use `dynamic` from `next/dynamic` with `ssr: false`.

```tsx
// app/labs/page.tsx or pages/labs.tsx
'use client';

import dynamic from 'next/dynamic';

const WebTerminal = dynamic(() => import('@/components/Terminal'), {
  ssr: false,
  loading: () => (
    <div className="w-full h-96 bg-[#090D16] border border-slate-800 rounded-xl flex items-center justify-center text-slate-400">
      Loading interactive shell...
    </div>
  ),
});

export default function LabPage() {
  return (
    <main className="max-w-4xl mx-auto p-6 space-y-4">
      <h1 className="text-2xl font-bold text-slate-900 dark:text-white">
        Linux Shell Basics
      </h1>
      <WebTerminal onCommand={(cmd) => console.log('Executed:', cmd)} />
    </main>
  );
}

```

---

### Step 4: Connecting to Real Linux Containers (Backend Integration)

For actual interactive Linux practice (beyond local JS mocks), connect xterm.js to a real backend container over **WebSockets**:

```
[ Browser (xterm.js) ] <--- WebSocket ---> [ Next.js / Node.js API / Socket Server ] <--- Stream ---> [ Docker Container (node-pty) ]

```

1. **Backend Execution:** Use `node-pty` on a Node.js server to spawn an isolated pseudo-terminal inside a Docker container:
```javascript
const pty = require('node-pty');
const ptyProcess = pty.spawn('docker', ['run', '-it', '--rm', 'alpine', 'sh'], {
  name: 'xterm-color',
  cols: 80,
  rows: 24,
});

```


2. **WebSocket Relay:** Use `@xterm/addon-attach` in the frontend to bind terminal input directly to the WebSocket stream:
```typescript
import { AttachAddon } from '@xterm/addon-attach';

const ws = new WebSocket('wss://your-backend-api.com/shell');
const attachAddon = new AttachAddon(ws);
term.loadAddon(attachAddon);

```



---
