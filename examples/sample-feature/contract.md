# Contract: User profile page

## API endpoint

### GET /api/users/me

Returns the current authenticated user's profile.

**Headers:**

| Header | Value |
|---|---|
| `Authorization` | `Bearer <token>` |

**Response 200:**

```json
{
  "id": "string",
  "name": "string",
  "email": "string",
  "avatarUrl": "string | null",
  "createdAt": "string (ISO 8601)"
}
```

**Response 401:**

```json
{
  "error": "Unauthorised",
  "message": "Valid authentication token required."
}
```

## Component interface

```
ProfilePage
  Props: none (fetches own data)
  Emits: none
  Route: /profile
```

## Types

```
interface UserProfile {
  id: string
  name: string
  email: string
  avatarUrl: string | null
  createdAt: string
}
```
