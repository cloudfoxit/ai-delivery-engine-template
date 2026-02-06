# Open questions: User profile page

## OQ-1: Avatar upload

**Question:** Should the profile page include an avatar upload feature, or is that a separate feature?

**Status:** resolved

**Resolution:** Avatar upload is a separate feature. This feature only *displays* the existing avatar.

## OQ-2: Profile editing

**Question:** Should users be able to edit their name/email from this page?

**Status:** resolved

**Resolution:** No. Profile editing will be a follow-up feature. This page is read-only.

## OQ-3: Date localisation

**Question:** Should the "Joined X ago" text be localised to the user's locale?

**Status:** non-blocking

**Notes:** Nice to have but not required for the initial implementation. The Builder may use the library's default (English) locale. Localisation can be added in a follow-up.
