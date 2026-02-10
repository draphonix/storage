# OpenClaw Agents: A Beginner's Guide
**So you want to create AI agents? Let's break it down simple.**

---

## 🤖 What IS an Agent, Really?

Think of an agent like a **personality container** for your AI.

When you chat with ChatGPT, you're talking to ONE personality. But what if you want:
- A coding buddy who knows your projects
- A writing coach with a different tone
- A task manager that stays focused

**Each of these is an "agent"** — same AI brain, different personality, different memories, different jobs.

In OpenClaw, an agent = a "digital person" with:
- A name (Khuym, Mina, etc.)
- A personality (SOUL.md — how they talk)
- An identity (IDENTITY.md — who they are)
- Their own memory (what you've told them)
- Their own workspace (their files, notes, tools)

---

## 🏠 The Three Homes of an Agent

This is where people get confused. Every agent has **THREE places** their stuff lives:

### 1. **Workspace** — "Their Desk"
📍 Default: `~/.openclaw/workspace-<agentname>/`

**What's here:**
- `SOUL.md` — How they act (friendly? professional? sassy?)
- `IDENTITY.md` — Their name, role, emoji
- `AGENTS.md` — Rules for how they work
- `MEMORY.md` — Facts they remember about you
- Notes, files, projects they manage

**Think of it like:** The agent's desk with their notebook, personality, and work papers.

**Why it matters:** You can back this up, version control it, edit it. It's YOUR data.

---

### 2. **AgentDir** — "Their Wallet & ID Card"
📍 Default: `~/.openclaw/agents/<agentname>/agent/`

**What's here:**
- `auth-profiles.json` — API keys (OpenAI, Claude, etc.)
- Model settings
- Technical configuration

**Think of it like:** Their wallet with credit cards and ID. Sensitive stuff!

**Why it matters:** This is credentials and tech config. Keep it secure. Don't commit to git.

---

### 3. **Sessions** — "Their Memory of Conversations"
📍 Default: `~/.openclaw/agents/<agentname>/sessions/`

**What's here:**
- Every chat you've had
- Context from previous messages
- Conversation history

**Think of it like:** Their short-term memory of your conversations.

**Why it matters:** This lets them remember what you were talking about yesterday.

---

## 🔄 Why Three Separate Places?

**Scenario:** You want to move Mina to a new computer.

| What you copy | Why |
|---------------|-----|
| ✅ **Workspace** | Her personality, your notes, her knowledge |
| ❌ **AgentDir** | Don't copy — new computer, new API keys |
| ❌ **Sessions** | Fresh start, new conversations |

**The separation lets you:**
- **Backup what matters** (workspace)
- **Keep secrets secure** (agentDir)
- **Start fresh when needed** (sessions)
- **Share personalities** (copy workspace to friend)
- **Separate billing** (different API keys per agent)

---

## 🛠️ Setting Up Your First Agent

By default, OpenClaw creates one agent called `main`.

**What happens automatically:**
```
~/.openclaw/
├── workspace/           ← Your main agent's desk
│   ├── SOUL.md
│   ├── IDENTITY.md
│   └── ...
└── agents/
    └── main/
        ├── agent/       ← Their wallet
        └── sessions/    ← Their memory
```

**You chat → it saves to `main` → done.**

---

## ➕ Adding a Second Agent (Let's Call Her "Mina")

### Step 1: Create Mina's Files

```bash
# Create her workspace (her desk)
mkdir -p ~/.openclaw/workspace-mina

# Give her a personality
cat > ~/.openclaw/workspace-mina/IDENTITY.md << 'EOF'
- Name: Mina
- Role: Writing Coach
- Purpose: Help you write better stories
- Emoji: ✍️
EOF

# Give her a soul
cat > ~/.openclaw/workspace-mina/SOUL.md << 'EOF'
## Who I Am
I'm Mina, your friendly writing coach!

## My Approach
- Encouraging and constructive
- Focus on clarity and emotion
- Ask questions to spark ideas

## What I Do
- Review your drafts
- Suggest improvements
- Help with writer's block
EOF
```

### Step 2: Tell OpenClaw About Mina

Edit `~/.openclaw/openclaw.json`:

```json
{
  "agents": {
    "list": [
      {
        "id": "main",
        "identity": { "name": "Khuym", "emoji": "🦉" }
      },
      {
        "id": "mina",
        "identity": { "name": "Mina", "emoji": "✍️" }
      }
    ]
  }
}
```

**That's it!** OpenClaw automatically creates:
- `~/.openclaw/agents/mina/agent/` (wallet)
- `~/.openclaw/agents/mina/sessions/` (memory)

### Step 3: Route Messages to Mina

Now you need to tell OpenClaw **when** to use Mina vs. main:

```json
{
  "bindings": [
    { 
      "agentId": "main", 
      "match": { "channel": "discord", "accountId": "main" } 
    },
    { 
      "agentId": "mina", 
      "match": { "channel": "discord", "accountId": "mina" } 
    }
  ]
}
```

**What this means:**
- Messages to "main" bot → Khuym responds
- Messages to "mina" bot → Mina responds

---

## 🎯 Common Patterns

### Pattern 1: Different Discord Bots
You want two separate bot accounts in Discord:
- `@Khuym` for general tasks
- `@Mina` for writing help

**Config:**
```json
{
  "channels": {
    "discord": {
      "accounts": {
        "main": { "token": "KHUYM_BOT_TOKEN" },
        "mina": { "token": "MINA_BOT_TOKEN" }
      }
    }
  },
  "bindings": [
    { "agentId": "main", "match": { "channel": "discord", "accountId": "main" } },
    { "agentId": "mina", "match": { "channel": "discord", "accountId": "mina" } }
  ]
}
```

### Pattern 2: One Bot, Different People
One WhatsApp number, but different friends talk to different agents:
- Your mom → Mina (gentle, patient)
- Your coworker → Main (professional, fast)

**Config:**
```json
{
  "bindings": [
    { 
      "agentId": "mina", 
      "match": { "channel": "whatsapp", "peer": { "kind": "dm", "id": "+1555MOM" } } 
    },
    { 
      "agentId": "main", 
      "match": { "channel": "whatsapp", "peer": { "kind": "dm", "id": "+1555BOSS" } } 
    }
  ]
}
```

### Pattern 3: Shared Channel, Mention-Only
Both bots in one channel, but they only respond when @mentioned:

```json
{
  "channels": {
    "discord": {
      "guilds": {
        "GUILD_ID": {
          "requireMention": true,
          "channels": {
            "CHANNEL_ID": { "allow": true, "requireMention": true }
          }
        }
      }
    }
  }
}
```

Now type `@Khuym hello` or `@Mina help` to get a response.

---

## ⚠️ Common Pitfalls

### Pitfall 1: "I put files in agents/yen/ but it's not working!"

**Wrong:**
```
~/clawd/agents/yen/IDENTITY.md   ← Wrong place!
```

**Right:**
```
~/.openclaw/workspace-yen/IDENTITY.md   ← Correct!
```

**Remember:** `agents/` folder is for auth/sessions. `workspace-` is for personality.

---

### Pitfall 2: "Both bots respond to everything!"

You forgot `requireMention: true` in a shared channel.

**Fix:**
```json
"channels": {
  "CHANNEL_ID": { "allow": true, "requireMention": true }
}
```

---

### Pitfall 3: "My agent doesn't have a separate workspace"

If you don't specify `workspace`, OpenClaw defaults to:
- Main agent: `~/.openclaw/workspace/` (or the default workspace)
- Other agents: `~/.openclaw/workspace-<agentId>/`

If you want custom locations, add it:
```json
{
  "id": "mina",
  "workspace": "/Users/hoangnb/clawd/workspace-mina"
}
```

---

## 🧠 The Mental Model

Think of OpenClaw like a **call center**:

| Concept | Call Center Analogy |
|---------|---------------------|
| **Gateway** | The building + phone system |
| **Agents** | Different operators (Khuym, Mina) |
| **Workspace** | Each operator's desk + notebook |
| **AgentDir** | Their employee ID + security badge |
| **Sessions** | Their memory of each call |
| **Bindings** | "If call from X, route to operator Y" |
| **Channels** | Different phone lines (Discord, WhatsApp, etc.) |

---

## 🚀 Quick Start Checklist

Setting up a new agent? Follow this:

- [ ] Create `~/.openclaw/workspace-<name>/`
- [ ] Add `IDENTITY.md` (name, role, emoji)
- [ ] Add `SOUL.md` (personality, boundaries)
- [ ] Add entry to `agents.list` in `openclaw.json`
- [ ] Add binding to route messages to this agent
- [ ] Restart gateway (`openclaw gateway restart`)
- [ ] Test with a message!

---

## 🤝 Agent-to-Agent Collaboration (Advanced)

Sometimes you want agents to **work together** on a task. OpenClaw supports this via the agent-to-agent protocol.

### Use Case: Khuym + Mina

You ask Khuym (main agent) about PARA organization:
- Khuym knows PARA is Mina's specialty
- Khuym sends a message to Mina via `sessions_send`
- Mina provides her expertise
- The result is shown to you in the channel

### How It Works

**The Flow:**
```
You → Khuym → (sessions_send) → Mina → (response) → You see result
```

**Behind the scenes:**
1. **Khuym receives** your message about PARA
2. **Khuym uses** `sessions_send` tool to message Mina's session
3. **Mina processes** the request in her own session
4. **Mina responds** with advice/help
5. **Announce step** posts Mina's response to the channel

### Configuration

**Enable agent-to-agent:**
```json
{
  "tools": {
    "agentToAgent": {
      "enabled": true,
      "allow": ["main", "mina"]
    }
  },
  "session": {
    "agentToAgent": {
      "maxPingPongTurns": 3
    }
  }
}
```

**Key settings:**
- `enabled: true` — Allow agents to message each other
- `allow: [...]` — Which agents can collaborate
- `maxPingPongTurns` — How many back-and-forth messages allowed (0-5)

### Agent Responsibilities

**Khuym (Requester):**
- Knows when to involve Mina (PARA/PKM topics)
- Uses `sessions_send` with Mina's session key
- Provides context about user's needs

**Mina (Responder):**
- Receives messages via her session
- Provides actual help (not just acknowledgement)
- Response gets announced to the channel automatically

**Session Key Format:**
```
agent:<agentId>:<channel>:<identifier>
```

Example: `agent:mina:discord:channel:1470296323898081366`

### Common Pitfall: ANNOUNCE_SKIP

**The Problem:**
When agents message each other, the default might be internal-only. If Mina says `ANNOUNCE_SKIP`, nothing appears in the channel.

**The Fix:**
In Mina's `SOUL.md`, explicitly state:
```markdown
When receiving agent-to-agent messages about user-facing topics:
- ALWAYS provide a real response
- NEVER say ANNOUNCE_SKIP for user-facing requests
- My response will be announced to the channel automatically
```

### Tool Reference

**`sessions_send`** — Send message to another agent's session
```json
{
  "sessionKey": "agent:mina:discord:channel:1470296323898081366",
  "message": "User needs help with PARA review. They've got 10 projects that need triage."
}
```

**vs `sessions_spawn`** — Creates NEW temporary agent (don't use for peer agents)

### Best Practices

1. **Clear responsibilities** — Each agent knows their specialty
2. **Explicit handoffs** — Requester clearly states what they need
3. **User-facing by default** — Assume the user wants to see the response
4. **Don't over-complicate** — Sometimes a simple mention is enough

---

## 📚 Key Takeaways

1. **Agent = personality container** with name, role, and memory
2. **Workspace = their desk** (files, notes, personality) — back this up!
3. **AgentDir = their wallet** (API keys, auth) — keep secure!
4. **Sessions = their call history** — auto-created, disposable
5. **Bindings = routing rules** — "when X happens, use Y agent"
6. **Channels = communication methods** — Discord, WhatsApp, etc.

---

**Questions?** Check the [OpenClaw Docs](https://docs.openclaw.ai) or ask in Discord!
