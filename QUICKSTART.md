# Quick Start Guide

This is a quick reference for getting started with your engineering notes repository.

## Creating Your First Note

### Option 1: Copy a Template
```bash
# For a daily note
cp templates/daily-template.md notes/daily/2025-11-21.md

# For a meeting note
cp templates/meeting-template.md notes/meetings/2025-11-21-team-sync.md

# For a research note
cp templates/research-template.md notes/research/2025-11-21-topic.md

# For a project note
cp templates/project-template.md notes/projects/my-project.md
```

### Option 2: Use Command Line
```bash
# Create a new daily note for today
cat templates/daily-template.md | sed "s/YYYY-MM-DD/$(date +%Y-%m-%d)/" > notes/daily/$(date +%Y-%m-%d).md

# Edit the file
vim notes/daily/$(date +%Y-%m-%d).md
# or
code notes/daily/$(date +%Y-%m-%d).md
```

## Daily Workflow

### Morning
1. Create today's daily note (if not done automatically)
2. Fill in "Today's Goals" section
3. Check yesterday's action items

### Throughout the Day
- Update work log as you complete tasks
- Document meetings with detailed notes
- Record learnings and blockers
- Create research notes for investigations

### End of Day
1. Mark completed tasks
2. Document key learnings
3. Note any blockers
4. Set tomorrow's priorities
5. Commit and push your notes

```bash
git add notes/
git commit -m "Daily notes for $(date +%Y-%m-%d)"
git push
```

## Common Commands

### Search for content
```bash
# Find all notes mentioning "API"
git grep "API"

# Search in specific directory
git grep "database" notes/research/

# Case-insensitive search
git grep -i "meeting"
```

### View recent notes
```bash
# List recent daily notes
ls -lt notes/daily/ | head -5

# Show recent commits
git log --oneline --since="1 week ago"
```

### Link between notes
In your markdown, reference other notes:
```markdown
See [project documentation](../projects/my-project.md) for more details.
Related: [yesterday's notes](./2025-11-20.md)
```

## Tips for Effective Note-Taking

1. **Write notes immediately** - Don't wait until later
2. **Use checklists** - Great for action items and tasks
3. **Link related content** - Create connections between notes
4. **Review regularly** - Go back and update project notes
5. **Be concise** - Capture key information, not everything
6. **Use tags** - Makes searching easier later
7. **Date everything** - Always include dates in headers

## Automation Ideas

### Create Daily Note Script
Save as `create-daily-note.sh`:
```bash
#!/bin/bash
TODAY=$(date +%Y-%m-%d)
DAY_NAME=$(date +%A)
FILE="notes/daily/$TODAY.md"

if [ ! -f "$FILE" ]; then
    cp templates/daily-template.md "$FILE"
    # Replace date placeholders
    sed -i "s/YYYY-MM-DD/$TODAY/g" "$FILE"
    sed -i "s/Day of Week/$DAY_NAME/g" "$FILE"
    echo "Created $FILE"
    # Open in editor
    ${EDITOR:-vim} "$FILE"
else
    echo "Note already exists: $FILE"
fi
```

Make it executable:
```bash
chmod +x create-daily-note.sh
./create-daily-note.sh
```

### Commit Hook for Auto-commit
Save as `.git/hooks/pre-push`:
```bash
#!/bin/bash
# Ensure notes are committed before pushing
if [ -n "$(git status --porcelain notes/)" ]; then
    echo "Uncommitted notes found!"
    read -p "Commit notes now? (y/n) " -n 1 -r
    echo
    if [[ $REPLY =~ ^[Yy]$ ]]; then
        git add notes/
        git commit -m "Update notes $(date +%Y-%m-%d)"
    fi
fi
```

## IDE Integration

### VS Code
Install these extensions:
- Markdown All in One
- Markdown Preview Enhanced
- markdownlint

Add to `.vscode/settings.json`:
```json
{
    "files.defaultLanguage": "markdown",
    "markdown.preview.breaks": true,
    "[markdown]": {
        "editor.wordWrap": "on",
        "editor.quickSuggestions": true
    }
}
```

### Vim
Add to `.vimrc`:
```vim
" Markdown settings
autocmd FileType markdown setlocal spell
autocmd FileType markdown setlocal wrap
autocmd FileType markdown setlocal linebreak
```

## Mobile Access

Use these apps for mobile access:
- **GitHub Mobile** - View and edit directly on GitHub
- **Working Copy** (iOS) - Full Git client
- **Markor** (Android) - Markdown editor with Git
- **iA Writer** - Clean markdown writing experience

---

Happy note-taking! 📝
