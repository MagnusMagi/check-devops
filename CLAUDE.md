# check-devops

Public Claude Code skill + plugin (github.com/MagnusMagi/check-devops, MIT).

- Skill: skills/check-devops/{SKILL.md,checklist.md}. ~/.claude/skills/check-devops is a symlink here (local copy = repo).
- Plugin/marketplace manifests: .claude-plugin/{plugin.json,marketplace.json,icon.png}.
- Before commit: `claude plugin validate .`; bump `version` in plugin.json on skill changes.
- Images: assets/*.svg are sources; render with `ffmpeg -i assets/banner.svg assets/social-preview.png`
  (icon: `-vf scale=512:512 .claude-plugin/icon.png`). Social preview is uploaded by hand in GitHub Settings.
- Everything public-facing is English; reports follow the user's language.
