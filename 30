/*
# [Seeding Test Users]
This script inserts three test users into the authentication system. It uses the `auth.admin.create_user` function, which is the secure and recommended way to programmatically create users. This method also ensures that the `handle_new_user` trigger fires correctly, creating a corresponding profile for each user in the `public.profiles` table.

## Query Description:
This operation adds new records to the `auth.users` table and, by extension, the `public.profiles` table. It is non-destructive and does not affect existing user data. The users are created with pre-confirmed emails for ease of testing.

## Metadata:
- Schema-Category: ["Data"]
- Impact-Level: ["Low"]
- Requires-Backup: [false]
- Reversible: [true]

## Structure Details:
- Tables Affected: `auth.users`, `public.profiles`
- Operation: `INSERT` (via function call)

## Security Implications:
- RLS Status: [Not Applicable]
- Policy Changes: [No]
- Auth Requirements: [Requires admin privileges to run, which the Supabase SQL Editor provides.]

## Performance Impact:
- Indexes: [No change]
- Triggers: [Fires the `on_auth_user_created` trigger]
- Estimated Impact: [Negligible]
*/

-- Create three test users with pre-confirmed emails.
-- The password for all test users is: 'password123'

SELECT auth.admin.create_user(
  'test.user.one@example.com',
  'password123',
  '{"email_confirmed": true}'
);

SELECT auth.admin.create_user(
  'test.user.two@example.com',
  'password123',
  '{"email_confirmed": true}'
);

SELECT auth.admin.create_user(
  'test.user.three@example.com',
  'password123',
  '{"email_confirmed": true}'
);
