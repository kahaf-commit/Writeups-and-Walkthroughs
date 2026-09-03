# Claude Code

# API

```jsx
https://tabitoken.com/keys
```

## Install Claude Code

Open **PowerShell**:

```jsx
irm https://claude.ai/install.ps1 | iex
```

Then reopen PowerShell and verify:

```jsx
claude --version
```

One Liner:

```jsx
irm https://claude.ai/install.ps1 | iex; mkdir "$HOME\Desktop\BugBounty_Recon" -Force; cd "$HOME\Desktop\BugBounty_Recon"; claude
```

# Create `/sub-enum`

Run:

```jsx
New-Item -ItemType Directory -Force ".\.claude\skills\sub-enum"
```