## Basic Rules

| Element        | Style         | Examples                                          |
| -------------- | ------------- | ------------------------------------------------- |
| Buttons        | Sentence Case | `Create product`, `Save changes`, `Sign in`       |
| Labels         | Title Case    | `Email Address`, `First Name`, `Product Category` |
| Page Titles    | Title Case    | `User Settings`, `Product Details`                |
| Table Headers  | Title Case    | `Product Name`, `Created At`, `Status`            |
| Dropdown Menu  | Sentence case | `Edit profile`, `Delete item`, `View details`     |
| Checkbox/Radio | Sentence case | `Remember me`, `Send notifications`               |
| Toast/Alerts   | Sentence case | `Product created successfully`                    |

## Title Case

Capitalize first letter of each major word.
```jsx
<Button>Create Product</Button>
<Label>Email Address</Label>
<h1>User Settings</h1>
```

## Sentence Case

Capitalize only first letter of first word.
```jsx
<DropdownMenuItem>Edit profile</DropdownMenuItem>
<Checkbox>Remember me</Checkbox>
<Toast>Changes saved successfully</Toast>
```

## Exceptions: Brands & Acronyms

Always preserve proper casing:

### Acronyms
- API, ID, URL, PDF, CSV, JSON, HTML, CSS, SQL, OAuth

### Brands
- GitHub, YouTube, LinkedIn, iPhone, macOS, Next.js, TypeScript

### Examples
```jsx
// ✅ Correct
<DropdownMenuItem>Connect to GitHub</DropdownMenuItem>
<DropdownMenuItem>Export as PDF</DropdownMenuItem>
<DropdownMenuItem>View API docs</DropdownMenuItem>

// ❌ Wrong
<DropdownMenuItem>Connect to github</DropdownMenuItem>
<DropdownMenuItem>Export as pdf</DropdownMenuItem>
<DropdownMenuItem>View api docs</DropdownMenuItem>
```

## Real Example
```jsx
<DropdownMenu>
  <DropdownMenuTrigger>
    <Button>Options</Button> {/* Title Case */}
  </DropdownMenuTrigger>
  <DropdownMenuContent>
    <DropdownMenuLabel>Actions</DropdownMenuLabel> {/* Title Case */}
    <DropdownMenuItem>Edit profile</DropdownMenuItem> {/* Sentence */}
    <DropdownMenuItem>View API keys</DropdownMenuItem> {/* Preserve API */}
    <DropdownMenuItem>Connect GitHub</DropdownMenuItem> {/* Preserve brand */}
    <DropdownMenuSeparator />
    <DropdownMenuItem>Sign out</DropdownMenuItem> {/* Sentence */}
  </DropdownMenuContent>
</DropdownMenu>
```

## Why This Matters

- **Consistency** - Professional look & feel
- **Readability** - Sentence case easier to scan in menus
- **Industry Standard** - Used by GitHub, Linear, Notion, Gmail, etc.
- **Accessibility** - Better for screen readers

## Quick Reference

**Button/Label:** Think "heading" → Title Case
**Menu/Action:** Think "instruction" → Sentence case
**Always preserve:** Brand names & acronyms