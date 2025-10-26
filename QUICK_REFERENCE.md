# GitHub Copilot: Quick Reference Guide

A concise reference for GitHub Copilot features, shortcuts, and best practices.

---

## Installation (VS Code)

1. Install VS Code from [code.visualstudio.com](https://code.visualstudio.com/)
2. Open Extensions (`Ctrl+Shift+X` / `Cmd+Shift+X`)
3. Search "GitHub Copilot" and install both:
   - GitHub Copilot
   - GitHub Copilot Chat
4. Sign in to GitHub when prompted
5. Verify subscription is active

---

## Keyboard Shortcuts

| Action | Windows/Linux | macOS |
|--------|---------------|-------|
| **Accept suggestion** | `Tab` | `Tab` |
| **Reject suggestion** | `Esc` | `Esc` |
| **Next suggestion** | `Alt+]` | `Option+]` |
| **Previous suggestion** | `Alt+[` | `Option+[` |
| **Trigger Copilot** | `Alt+\` | `Option+\` |
| **Open Copilot Chat** | `Ctrl+Shift+I` | `Cmd+Shift+I` |
| **Inline chat** | `Ctrl+I` | `Cmd+I` |
| **Accept word-by-word** | `Ctrl+→` | `Cmd+→` |

---

## Chat Commands

| Command | Description |
|---------|-------------|
| `/explain` | Explain selected code |
| `/fix` | Fix problems in code |
| `/tests` | Generate unit tests |
| `/help` | Show available commands |
| `/clear` | Clear chat history |

---

## Best Practices

### ✅ Do's

1. **Write clear comments** before functions
   ```javascript
   // Calculate compound interest for given principal, rate, and time
   function calculateCompoundInterest(principal, rate, years) {
   ```

2. **Use descriptive names**
   ```python
   # Good
   def calculate_user_monthly_payment(principal, interest_rate):
   
   # Bad
   def calc(p, r):
   ```

3. **Provide context** - Open related files in your workspace

4. **Review suggestions** - Always verify generated code

5. **Iterate** - Cycle through suggestions with `Alt+]` / `Alt+[`

6. **Use Chat** for complex problems

### ❌ Don'ts

1. **Don't blindly accept** all suggestions
2. **Don't skip testing** generated code
3. **Don't ignore security** concerns
4. **Don't forget to review** for edge cases
5. **Don't use for** critical security code without verification

---

## Common Use Cases

### 1. Generate Function from Comment
```python
# Function to validate email address using regex
def validate_email(email):
    # Copilot generates the implementation
```

### 2. Complete Repetitive Code
```java
// After first getter/setter, Copilot suggests the rest
public String getName() { return name; }
public void setName(String name) { this.name = name; }
// Copilot continues with other fields
```

### 3. Generate Tests
```javascript
function add(a, b) { return a + b; }

// Write unit tests for the add function
describe('add', () => {
    // Copilot generates test cases
});
```

### 4. Explain Code
Select code → Open Chat → Type `/explain`

### 5. Fix Errors
Paste error message in Chat → Ask for solution

### 6. Refactor Code
Select code → Inline Chat (`Ctrl+I`) → "Refactor to use async/await"

---

## Language-Specific Tips

### JavaScript/TypeScript
- Use JSDoc comments for better suggestions
- Define interfaces/types for TypeScript
- Open related files for context

### Python
- Use docstrings
- Type hints improve suggestions
- Follow PEP 8 naming conventions

### Java
- Use JavaDoc comments
- Clear class/method names
- Define interfaces first

### React
- Use descriptive component names
- Comment component purpose
- Define PropTypes or TypeScript interfaces

---

## Troubleshooting

| Problem | Solution |
|---------|----------|
| No suggestions appearing | Check status bar icon, verify subscription, restart VS Code |
| Irrelevant suggestions | Write clearer comments, use better names, provide more context |
| Slow suggestions | Check internet connection, close heavy applications |
| Can't sign in | Clear browser cache, try different browser, revoke and re-authorize |
| Extension not activating | Reinstall extension, check Command Palette for errors |

---

## Settings

Access: `File → Preferences → Settings → Search "Copilot"`

**Useful Settings:**
- `github.copilot.enable`: Enable/disable Copilot
- `github.copilot.inlineSuggest.enable`: Enable inline suggestions
- `github.copilot.editor.enableAutoCompletions`: Auto-completions

---

## File Exclusion

Create `.copilotignore` in repository root:

```
# Exclude sensitive files
*.env
*.key
secrets/
config/production.js
.private/
```

---

## Efficiency Tips

### 1. Use Tab Completion Aggressively
Accept good suggestions immediately to maintain flow

### 2. Write Comments First
Describe what you want, then let Copilot implement

### 3. Leverage Chat for Planning
Ask for architecture advice before coding

### 4. Learn Patterns
Notice what prompts generate good suggestions

### 5. Combine with Manual Coding
Use Copilot for boilerplate, write critical logic yourself

### 6. Use Multi-File Context
Open related files to give Copilot more information

### 7. Iterate Quickly
Test suggestions immediately, refine if needed

---

## Example Workflows

### Workflow 1: New Feature
1. Write comment describing feature
2. Accept Copilot's function skeleton
3. Refine implementation
4. Ask Chat to generate tests
5. Review and commit

### Workflow 2: Bug Fix
1. Copy error message to Chat
2. Ask for explanation
3. Review suggested fix
4. Apply and test
5. Add error handling

### Workflow 3: Code Review
1. Select code block
2. Use `/explain` in Chat
3. Ask "How can this be improved?"
4. Apply suggestions
5. Test improvements

---

## Resources

- **Documentation**: [docs.github.com/copilot](https://docs.github.com/copilot)
- **VS Code Guide**: [code.visualstudio.com/docs/copilot](https://code.visualstudio.com/docs/copilot)
- **Community**: [github.community](https://github.community)
- **Blog**: [github.blog/tag/github-copilot](https://github.blog/tag/github-copilot)

---

## Pricing

- **Individual**: $10/month or $100/year
- **Students**: Free with GitHub Student Developer Pack
- **Open Source**: Free for verified maintainers
- **Business**: $19/user/month
- **Enterprise**: Contact sales

---

## Support

- Check status: [githubstatus.com](https://githubstatus.com)
- Report issues: [github.com/community](https://github.com/community)
- Get help: [github.com/support](https://github.com/support)

---

**Quick Start**: Install extension → Sign in → Start coding → Press `Tab` to accept suggestions → Use `Ctrl+Shift+I` for Chat

**Pro Tip**: Spend 30 minutes daily using Copilot to build muscle memory with shortcuts and workflows.
