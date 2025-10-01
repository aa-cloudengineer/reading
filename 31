/*
# [Add created_at to stories table]
This migration adds a `created_at` column to the `stories` table to track when each story was created. This aligns the database schema with the application's data model and allows for chronological sorting of stories.

## Query Description:
This is a non-destructive operation that adds a new column with a default value. It will not affect existing data in other columns. The `created_at` value for existing rows will be set to the time the migration is run.

## Metadata:
- Schema-Category: "Structural"
- Impact-Level: "Low"
- Requires-Backup: false
- Reversible: true (The column can be dropped)

## Structure Details:
- Table: `stories`
- Column Added: `created_at` (timestamptz, default now())

## Security Implications:
- RLS Status: Unchanged
- Policy Changes: No
- Auth Requirements: None

## Performance Impact:
- Indexes: None added. An index could be added later if sorting performance becomes an issue.
- Triggers: None
- Estimated Impact: Low.
*/

ALTER TABLE public.stories
ADD COLUMN created_at TIMESTAMPTZ DEFAULT now() NOT NULL;
