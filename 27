/*
          # [Initial Schema Setup]
          This migration sets up the core tables for the EchoRead application, including profiles, stories, and user_sessions. It also establishes Row Level Security (RLS) to protect user data and adds a trigger to automatically create user profiles upon signup.

          ## Query Description: [This operation will create new tables and enable security features. It is designed to run on a fresh database and should not affect any existing data if you have none. It is safe to apply.]
          
          ## Metadata:
          - Schema-Category: "Structural"
          - Impact-Level: "Low"
          - Requires-Backup: false
          - Reversible: true
          
          ## Structure Details:
          - Tables Created: `profiles`, `stories`, `user_sessions`
          - Functions Created: `handle_new_user`
          - Triggers Created: `on_auth_user_created`
          
          ## Security Implications:
          - RLS Status: Enabled on all new tables.
          - Policy Changes: Yes, new policies are created to ensure users can only access their own data.
          - Auth Requirements: Policies rely on `auth.uid()` to identify the current user.
          
          ## Performance Impact:
          - Indexes: Primary keys and foreign keys are indexed by default.
          - Triggers: A trigger is added to `auth.users` which runs once on user creation.
          - Estimated Impact: Low performance impact.
          */

-- 1. Create Profiles Table
-- This table stores public user data and is linked to the auth.users table.
CREATE TABLE public.profiles (
  id UUID NOT NULL PRIMARY KEY REFERENCES auth.users(id) ON DELETE CASCADE,
  updated_at TIMESTAMPTZ,
  username TEXT,
  level INT4 NOT NULL DEFAULT 1,
  xp INT4 NOT NULL DEFAULT 0,
  streak_days INT4 NOT NULL DEFAULT 0,
  total_reading_time INT4 NOT NULL DEFAULT 0,
  completed_sessions INT4 NOT NULL DEFAULT 0
);

-- Set up Row Level Security (RLS) for the profiles table
ALTER TABLE public.profiles ENABLE ROW LEVEL SECURITY;

CREATE POLICY "Users can view their own profile."
  ON public.profiles FOR SELECT
  USING (auth.uid() = id);

CREATE POLICY "Users can update their own profile."
  ON public.profiles FOR UPDATE
  USING (auth.uid() = id)
  WITH CHECK (auth.uid() = id);

-- 2. Create Function and Trigger for New User Profiles
-- This function automatically creates a profile for a new user upon signup.
CREATE OR REPLACE FUNCTION public.handle_new_user()
RETURNS TRIGGER AS $$
BEGIN
  INSERT INTO public.profiles (id, username)
  VALUES (new.id, new.raw_user_meta_data->>'username');
  RETURN new;
END;
$$ LANGUAGE plpgsql SECURITY DEFINER;

-- This trigger calls the function after a new user is inserted into auth.users.
CREATE TRIGGER on_auth_user_created
  AFTER INSERT ON auth.users
  FOR EACH ROW EXECUTE PROCEDURE public.handle_new_user();

-- 3. Create Stories Table
-- This table stores the reading materials for the app.
CREATE TABLE public.stories (
  id UUID NOT NULL PRIMARY KEY DEFAULT gen_random_uuid(),
  title TEXT NOT NULL,
  text TEXT NOT NULL,
  difficulty TEXT NOT NULL CHECK (difficulty IN ('beginner', 'intermediate', 'advanced'))
);

-- Set up RLS for the stories table
ALTER TABLE public.stories ENABLE ROW LEVEL SECURITY;

CREATE POLICY "Authenticated users can view all stories."
  ON public.stories FOR SELECT
  TO authenticated
  USING (true);

-- 4. Insert Initial Stories
-- Populate the stories table with the default reading content.
INSERT INTO public.stories (title, text, difficulty) VALUES
('The Little Red Hen', 'Once upon a time, there was a little red hen who lived on a farm. She was a very hardworking hen and always busy with her daily tasks. One day, while walking around the farmyard, she found some grains of wheat. "Who will help me plant this wheat?" asked the little red hen. The lazy cat said, "Not I." The sleepy dog said, "Not I." The noisy duck said, "Not I." "Then I will do it myself," said the little red hen. And she did.', 'beginner'),
('The Solar System Adventure', 'Our solar system consists of the Sun and all the celestial bodies that orbit around it. This includes eight planets, their moons, asteroids, comets, and other space debris. The Sun, which is a medium-sized star, provides the energy that makes life possible on Earth. The planets can be divided into two main groups: the inner rocky planets (Mercury, Venus, Earth, and Mars) and the outer gas giants (Jupiter, Saturn, Uranus, and Neptune). Each planet has unique characteristics that make it fascinating to study.', 'intermediate'),
('The Secrets of Climate Science', 'Climate change represents one of the most significant challenges facing humanity in the twenty-first century. The phenomenon is characterized by long-term alterations in global weather patterns, primarily attributed to increased concentrations of greenhouse gases in the atmosphere. These gases, including carbon dioxide, methane, and nitrous oxide, trap heat from the sun and cause the Earth''s average temperature to rise. The consequences of this warming trend are far-reaching and include rising sea levels, more frequent extreme weather events, shifts in precipitation patterns, and threats to biodiversity.', 'advanced');


-- 5. Create User Sessions Table
-- This table logs the reading sessions completed by users.
CREATE TABLE public.user_sessions (
  id UUID NOT NULL PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id UUID NOT NULL REFERENCES public.profiles(id) ON DELETE CASCADE,
  story_id UUID NOT NULL REFERENCES public.stories(id) ON DELETE CASCADE,
  score INT2,
  completed_at TIMESTAMPTZ NOT NULL DEFAULT now()
);

-- Set up RLS for the user_sessions table
ALTER TABLE public.user_sessions ENABLE ROW LEVEL SECURITY;

CREATE POLICY "Users can manage their own reading sessions."
  ON public.user_sessions FOR ALL
  USING (auth.uid() = user_id)
  WITH CHECK (auth.uid() = user_id);
