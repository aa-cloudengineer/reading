/*
# [Secure Function Search Path]
This migration secures the `handle_new_user` function by explicitly setting its `search_path`. This addresses a security advisory by preventing the function from being influenced by session-level search path modifications, ensuring it always operates on the intended `public` schema.

## Query Description: [This operation modifies an existing database function to enhance security. It ensures that the function for creating new user profiles always looks for tables in the 'public' schema first, which is a recommended security practice. There is no risk to existing data.]

## Metadata:
- Schema-Category: ["Structural"]
- Impact-Level: ["Low"]
- Requires-Backup: [false]
- Reversible: [true]

## Structure Details:
- Function `handle_new_user` is being altered.

## Security Implications:
- RLS Status: [N/A]
- Policy Changes: [No]
- Auth Requirements: [N/A]
- Mitigates: [WARN] Function Search Path Mutable

## Performance Impact:
- Indexes: [N/A]
- Triggers: [N/A]
- Estimated Impact: [None]
*/

ALTER FUNCTION public.handle_new_user() SET search_path = 'public';
