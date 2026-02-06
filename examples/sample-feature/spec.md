# Spec: User profile page

## Summary

Display a user profile page that shows the user's name, email, avatar, and account creation date. The page is accessible to authenticated users viewing their own profile.

## User stories

- As an authenticated user, I want to view my profile so that I can see my account information.
- As an authenticated user, I want to see when my account was created so that I know my tenure.

## Behaviour

- The profile page loads the current user's data from the API.
- If the API call fails, an error state is displayed with a retry option.
- If the user has no avatar, a default placeholder is shown.
- The account creation date is displayed in a human-readable relative format (e.g., "Joined 3 months ago").

## Edge cases

- **No avatar:** Show a generated placeholder using the user's initials.
- **API timeout:** Show an error message after 10 seconds with a retry button.
- **Unauthenticated access:** Redirect to the sign-in page.
