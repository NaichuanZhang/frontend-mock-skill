# Mock Marking Convention

## Tag Reference

Every mocked element in the codebase must be marked with a `@mock:` comment tag. This makes mocks searchable, removable, and documentable.

### Tags

| Tag | Purpose | When to Use |
|-----|---------|-------------|
| `@mock:data` | Fake data objects and fixtures | Any hardcoded data that would come from a database or API in production |
| `@mock:api` | Mock API endpoints | Routes or handlers that return fake responses |
| `@mock:asset` | Placeholder images, videos, icons | Any media URL pointing to a placeholder service or inline SVG substitute |
| `@mock:component` | Mock-only UI components | Components that exist solely for demonstration and have no production equivalent |
| `@mock:content` | Placeholder text and copy | Headlines, descriptions, paragraphs that a copywriter or product team would write |

### Format

Every `@mock:` comment follows this pattern:

```
// @mock:<tag> - <what to replace it with>
```

The description after the dash is important — it tells the developer (or the removal guide generator) what real data should go here.

### Placement Rules

1. **Place the tag on the line immediately above** the mocked element, not inline
2. **One tag per mock boundary** — if a whole data file is mock, tag it at the top; don't tag every line
3. **Be specific in the description** — "Replace with real user data" is okay; "Replace with data from the /api/v2/users endpoint after auth integration" is better
4. **Tag the source, not every consumer** — if `mockUsers` is defined once and used in 5 components, tag the definition, not every import

### Examples by Category

#### @mock:data
```tsx
// @mock:data - Replace with real user data from authentication provider
export const mockUsers: User[] = [
  { id: "u1", name: "Priya Sharma", email: "priya@example.com", avatar: "/avatars/1.jpg" },
  { id: "u2", name: "Marcus Johnson", email: "marcus@example.com", avatar: "/avatars/2.jpg" },
  { id: "u3", name: "Yuki Tanaka", email: "yuki@example.com", avatar: "/avatars/3.jpg" },
];

// @mock:data - Replace with real-time metrics from analytics service
export const dashboardMetrics = {
  totalRevenue: 248_750,
  activeUsers: 2847,
  conversionRate: 3.2,
  avgSessionDuration: "4m 32s",
};
```

#### @mock:api
```tsx
// @mock:api - Replace with real user management API
export async function GET(request: Request) {
  const { searchParams } = new URL(request.url);
  const page = parseInt(searchParams.get("page") || "1");

  // Return paginated mock data
  const start = (page - 1) * 10;
  const users = mockUsers.slice(start, start + 10);

  return Response.json({
    users,
    total: mockUsers.length,
    page,
    totalPages: Math.ceil(mockUsers.length / 10),
  });
}
```

#### @mock:asset
```tsx
// @mock:asset - Replace with actual product photography from CDN
const productImages = [
  "https://picsum.photos/seed/prod1/400/400",
  "https://picsum.photos/seed/prod2/400/400",
  "https://picsum.photos/seed/prod3/400/400",
];

// @mock:asset - Replace with company logo from brand assets folder
<img src="https://placehold.co/180x48/1a1a2e/ffffff?text=BrandLogo" alt="Logo" />
```

#### @mock:component
```tsx
// @mock:component - Remove in production; replace with real notification system or delete entirely
function DemoBanner() {
  return (
    <div className="bg-amber-50 border-b border-amber-200 px-4 py-2 text-center text-sm text-amber-800">
      This is a demo environment with sample data
    </div>
  );
}
```

#### @mock:content
```tsx
// @mock:content - Replace with real marketing copy from content team
<h1>Transform your workflow with intelligent automation</h1>
<p>
  Our platform helps teams of all sizes streamline their operations,
  reduce manual work, and focus on what matters most.
</p>
```

## Scanning for Mocks

To find all mock markers in a project:

```bash
# Find all mock tags
grep -rn "@mock:" --include="*.ts" --include="*.tsx" --include="*.js" --include="*.jsx" --include="*.css" .

# Count by category
grep -roh "@mock:[a-z]*" --include="*.ts" --include="*.tsx" --include="*.js" --include="*.jsx" . | sort | uniq -c | sort -rn

# Find only a specific category
grep -rn "@mock:api" --include="*.ts" --include="*.tsx" .
```

These commands are used by the removal guide generator to build the comprehensive `remove-mock-guide.md`.
