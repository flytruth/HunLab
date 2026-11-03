# 🛡️ HunLab — Next-Gen Cybersecurity Learning Platform

**Master Linux, Python, SOC Operations, and Red/Blue Team Tactics with Hands-On Labs.**

[Live Demo](https://hunlab.dev) · [Report Bug](https://github.com/hunlab/hunlab/issues) · [Request Feature](https://github.com/hunlab/hunlab/issues)

## ⚡ Overview

**HunLab** is a clean, modern, and highly interactive cybersecurity education platform built to bridge the gap between theoretical knowledge and real-world security operations. Designed with a clean slate aesthetic, subtle light-blue (`#38BDF8`) and lime-green (`#A3E635`) accents, and zero cheap neon clutter.

```
       [ Interactive Terminal ]           [ 100-Q CSA Exam Engine ]           [ Dynamic Roadmaps ]
       ┌──────────────────────┐           ┌───────────────────────┐           ┌──────────────────┐
       │ In-Browser xterm.js  │    ───►   │ Automated Explanations│    ───►   │ Inspired by      │
       │ Docker & WASM Ready  │           │ Detailed Metrics      │           │ roadmap.sh       │
       └──────────────────────┘           └───────────────────────┘           └──────────────────┘

```

## ✨ Features & Architecture

## 💎 Clear Access Tiers

HunLab offers tiered access structured for individual learners, SOC analysts, and enterprise security teams:

| Tier | Price | Linux & Bash | CSA Exam Engine | Interactive Labs | AI Tutor Quota | 
| ----- | ----- | ----- | ----- | ----- | ----- | 
| **Free** | `$0` | Basic | 10 Preview Qs | Web Mock | 3 Prompts/day | 
| **VIP** | `$9.99/mo` | Full | Full 100 Qs | Standard | Unlimited | 
| **Admin** | `$24.99/mo` | Full | Full + PDF Export | Custom Tools | Unlimited | 
| **Mega** | `$49.99/mo` | Full | Full + Benchmarks | Priority Cloud | Priority AI | 
| **Prince** | `$99.99` | Full | Lifetime Access | Dedicated Pods | VIP 1-on-1 | 

## 🚀 Tech Stack

* **Framework:** [Next.js 14](https://nextjs.org/) (App Router)

* **Styling:** [Tailwind CSS](https://tailwindcss.com/) & [Shadcn UI](https://ui.shadcn.com/)

* **Terminal Engine:** [`@xterm/xterm`](https://xtermjs.org/) & [`@xterm/addon-fit`](https://www.npmjs.com/package/@xterm/addon-fit)

* **State & Auth:** Clerk Auth / Supabase

* **Payments:** Stripe & LemonSqueezy Integrations

## 🛠️ Quick Start

### Prerequisites

* **Node.js** >= 18.x

* **npm** or **pnpm**

### Installation

1. **Clone the Repository**

   ```
   git clone https://github.com/hunlab/hunlab.git
   cd hunlab
   
   ```

2. **Install Dependencies**

   ```
   npm install
   
   ```

3. **Set Up Environment Variables**
   Create a `.env.local` file in the root directory:

   ```
   NEXT_PUBLIC_SITE_URL=http://localhost:3000
   NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY=your_stripe_key
   
   ```

4. **Run Development Server**

   ```
   npm run dev
   
   ```

   Open <http://localhost:3000> in your browser to view the app.

## 🤝 Contributing

Contributions are what make the open-source community such an amazing place to learn, inspire, and create. Any contributions you make are **greatly appreciated**.

1. Fork the Project

2. Create your Feature Branch (`git checkout -b feature/AmazingFeature`)

3. Commit your Changes (`git commit -m 'Add some AmazingFeature'`)

4. Push to the Branch (`git push origin feature/AmazingFeature`)

5. Open a Pull Request

## 📄 License

Distributed under the MIT License. See `LICENSE` for more information.
