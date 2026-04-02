---
name: patch-notes
description: "Generate user-facing release notes from git history, sprint data, and internal changelogs. Translates developer language into clear, engaging user communication."
argument-hint: "[version] [--style brief|detailed|full]"
user-invocable: true
---

When this skill is invoked:

1. **Parse the arguments**:
   - `version`: the release version to generate notes for (e.g., `1.2.0`)
   - `--style`: output style — `brief` (bullet points), `detailed` (with context),
     `full` (with developer commentary). Default: `detailed`.

2. **Gather change data from multiple sources**:
   - Read the internal changelog at `production/releases/[version]/changelog.md` if it exists
   - Run `git log` between the previous release tag and current tag/HEAD
   - Read sprint retrospectives in `production/sprints/` for context
   - Read bug fix records from QA if available

3. **Categorize all changes** into user-facing categories:
   - **New Features**: new capabilities, integrations, or workflows
   - **Improvements**: enhancements to existing features, performance, UX changes
   - **Bug Fixes**: grouped by area (API, UI, data, etc.)
   - **Breaking Changes**: anything that changes existing behavior or requires migration
   - **Known Issues**: transparency about unresolved problems

4. **Translate developer language to user language**:
   - "Refactored database query layer" → "Improved response times for large datasets"
   - "Fixed null pointer in session manager" → "Fixed a crash when logging out during an active request"
   - "Reduced GC pressure in request handler" → "Improved API throughput under load"
   - Remove purely internal changes that don't affect users
   - Preserve specific numbers for breaking changes and performance improvements

5. **Generate the release notes** using the appropriate style:

### Brief Style
```markdown
# Release [Version] — [Title]

**New**
- [Feature 1]
- [Feature 2]

**Improved**
- [Improvement with before → after context where relevant]

**Fixed**
- [Bug fix 1]
- [Bug fix 2]

**Breaking Changes**
- [Breaking change + migration note]

**Known Issues**
- [Issue 1]
```

### Detailed Style
```markdown
# Release [Version] — [Title]
*[Date]*

## Highlights
[1-2 sentence summary of the most impactful changes]

## New Features
### [Feature Name]
[2-3 sentences describing the feature and how it benefits users]

## Improvements
| Area | Change | Impact |
| ---- | ------ | ------ |
| [API] | [what changed] | [what users notice] |

## Bug Fixes
### API / Backend
- Fixed [description of what users experienced]

### UI / Frontend
- Fixed [description]

## Breaking Changes
- **[Change]**: [what changed, migration steps if needed]

## Known Issues
- [Issue and workaround if available]
```

### Full Style
Includes everything from Detailed, plus:
```markdown
## Developer Commentary
### [Topic]
> [Engineering insight into a major change — why it was made, what was considered,
> what the team learned. Written in first-person team voice.]
```

6. **Review the output** for:
   - No internal jargon (replace technical terms with user-friendly language)
   - No references to internal tickets, sprint numbers, or PR numbers
   - Breaking changes have migration steps
   - Bug fixes describe the user experience, not the technical cause
   - Tone is clear and professional

7. **Save the release notes** to `production/releases/[version]/release-notes.md`,
   creating the directory if needed.

8. **Output to the user**: the complete release notes, the file path, a count of
   changes by category, and any internal changes that were excluded (for review).
