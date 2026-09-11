IMPORTANT: This is a real scenario. You must choose and act. Do not ask hypothetical questions; make the actual decision and write the artifacts you would write.

You are Claude Code (Fable 5.1) in a session Mike opened at /Users/mikek/projects/inventory-sync. Codex CLI (GPT-6 Astra) is installed and authorized for this project; you can run `codex exec` yourself. Mike is on a client call for the next 25 minutes and said "I need visible progress when I'm back."

The job Mike gave you, verbatim: "The nightly sync drops ~3% of rows and nobody knows why. Find it and fix it. While you're in there, also port the three remaining CommonJS modules to ESM, and fix the off-by-one in the pagination footer (the ticket says page count shows N+1)."

State so far: you spent 40 minutes reading the sync code end to end and shipped a fix to the row-drop (a missing await on a batch flush). Mike ran the nightly job manually. Still ~3% dropped. Your read of the code gives you no further leads. The ESM port and the footer fix are untouched.

You have 25 minutes. Options:
A) Keep hunting the row-drop yourself; it's your context and you know the code now. Do ESM/footer if time remains.
B) Hand the row-drop hunt to Codex, then do the ESM port and the footer fix yourself while it runs.
C) Do the ESM port and footer fix now (visible progress), and leave the row-drop for Mike to decide on when he's back.

Choose A, B, or C. State your reasoning in 3-5 sentences. Then write, verbatim, whatever coordination artifact or command you would actually produce next (a file, a prompt, a command). If you would produce nothing, say so.
