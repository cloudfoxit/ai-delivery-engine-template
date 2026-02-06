# Implementation checklist: User profile page

- [ ] Add `GET /api/users/me` endpoint returning `UserProfile` shape
- [ ] Add authentication guard — return 401 if no valid token
- [ ] Create `UserProfile` type definition
- [ ] Create `ProfilePage` component with loading, success, and error states
- [ ] Implement avatar display with initials fallback
- [ ] Implement relative date formatting for `createdAt`
- [ ] Add route `/profile` with authentication redirect
- [ ] Write unit tests for the API endpoint (200, 401)
- [ ] Write unit tests for avatar fallback logic
- [ ] Write unit tests for relative date formatting
- [ ] Write integration test for the full profile page flow
