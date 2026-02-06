# Acceptance criteria: User profile page

## AC-1: Profile displays user information

**Given** an authenticated user navigates to `/profile`
**When** the page loads successfully
**Then** the user's name, email, avatar, and join date are displayed

## AC-2: Default avatar for users without one

**Given** an authenticated user with no avatar (`avatarUrl` is null)
**When** the profile page loads
**Then** a placeholder avatar with the user's initials is shown

## AC-3: Relative join date

**Given** the user's `createdAt` is "2025-11-01T00:00:00Z" and today is "2026-02-01"
**When** the profile page renders the join date
**Then** it displays "Joined 3 months ago"

## AC-4: Error state with retry

**Given** the API call to `/api/users/me` fails or times out
**When** the profile page handles the error
**Then** an error message is displayed with a "Retry" button that re-fetches the data

## AC-5: Unauthenticated redirect

**Given** an unauthenticated user navigates to `/profile`
**When** the page detects no valid session
**Then** the user is redirected to the sign-in page
