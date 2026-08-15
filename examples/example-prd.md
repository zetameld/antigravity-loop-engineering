---
type: prd
id: FEAT-001
title: User Authentication — Email + Password
depends_on: []
---

# PRD: User Authentication — Email + Password

## Goals
Allow users to register with email + password, sign in, and sign out.
Success metric: < 5% drop-off on registration form.

## User Flows

### Registration
1. User visits `/register`
2. Enters name, email, password (min 8 chars, 1 uppercase, 1 number)
3. Submits → server validates → creates account → redirects to dashboard
4. If email already taken → inline error under email field

### Sign In
1. User visits `/login`
2. Enters email + password → server validates → sets session cookie
3. Invalid creds → generic error ("Email or password incorrect") — do NOT
   distinguish which field failed (security)
4. After 5 failed attempts → 15-minute lockout

### Sign Out
1. User clicks "Sign out" → session destroyed → redirect to `/login`

## Acceptance Criteria
- [ ] Registration creates a hashed-password user record (bcrypt, ≥12 rounds)
- [ ] Login sets an httpOnly, SameSite=Strict, Secure session cookie
- [ ] Rate limiting: max 5 login attempts per IP per 15 minutes
- [ ] Password never logged or returned in any API response
- [ ] Registration form: all fields have visible labels, inline error messages
- [ ] Keyboard-navigable; focus management after error
- [ ] Responsive: works on 375px and 1280px

## Out of Scope
- OAuth / social login (separate PRD)
- Password reset flow (separate PRD)
- MFA (separate PRD)
