/*
          # [Data] Add More Stories
          [This operation inserts new reading passages into the 'stories' table to expand the content library for users.]

          ## Query Description: [This script adds 9 new stories across three difficulty levels (beginner, intermediate, advanced). This is a safe, non-destructive operation that only adds new rows. Existing data will not be affected.]
          
          ## Metadata:
          - Schema-Category: "Data"
          - Impact-Level: "Low"
          - Requires-Backup: false
          - Reversible: true
          
          ## Structure Details:
          - Table: public.stories
          - Columns Affected: id, title, text, difficulty, created_at
          
          ## Security Implications:
          - RLS Status: Enabled (No changes to policies)
          - Policy Changes: No
          - Auth Requirements: None for this migration.
          
          ## Performance Impact:
          - Indexes: None
          - Triggers: None
          - Estimated Impact: Negligible.
          */

-- Beginner Stories
INSERT INTO public.stories (id, title, text, difficulty, created_at) VALUES
(uuid_generate_v4(), 'The Kind Ant', 'A small ant saw a big leaf. The leaf was on the water. The ant was thirsty. It climbed on the leaf to drink. The wind blew. The leaf moved. The ant was safe. It was a good day.', 'beginner', now()),
(uuid_generate_v4(), 'My Red Ball', 'I have a red ball. It is big and round. I like to play with my ball in the park. My dog, Spot, likes to chase the ball. We run and jump. It is a lot of fun.', 'beginner', now()),
(uuid_generate_v4(), 'The Bright Sun', 'The sun is in the sky. It is yellow and bright. The sun gives us light and heat. Birds sing when the sun is up. Flowers open to see the sun. I like sunny days.', 'beginner', now());

-- Intermediate Stories
INSERT INTO public.stories (id, title, text, difficulty, created_at) VALUES
(uuid_generate_v4(), 'The Lost Compass', 'Leo was hiking in the forest. He checked his map and then reached for his compass, but it was gone. He felt a little worried as the sun began to set. Instead of panicking, he remembered his guide’s advice: look for moss on the trees, as it often grows on the north side. Following this natural sign, he slowly found his way back to the trail just as dusk settled.', 'intermediate', now()),
(uuid_generate_v4(), 'A City of Gardens', 'Maria visited a city famous for its beautiful gardens. Each park was a splash of color, with tulips, roses, and daisies arranged in amazing patterns. Fountains danced in the sunlight, and stone pathways invited her to explore. She learned that the city planners believed green spaces were essential for happiness and well-being.', 'intermediate', now()),
(uuid_generate_v4(), 'The Clever Robot', 'Unit 734 was a small cleaning robot. Its main job was to sweep floors. One day, it detected a water leak under a sink. Its programming didn''t include plumbing, but it could communicate. It sent a distress signal to the building manager’s computer, including a photo of the leak from its camera. The quick thinking of the little robot saved the building from a major flood.', 'intermediate', now());

-- Advanced Stories
INSERT INTO public.stories (id, title, text, difficulty, created_at) VALUES
(uuid_generate_v4(), 'The Quantum Entanglement Dilemma', 'Dr. Aris Thorne stared at the fluctuating data on his screen. His experiment had finally demonstrated quantum entanglement over a macroscopic distance, a feat previously confined to theory. However, the implications were staggering. If information could be transmitted instantaneously, it would violate the principle that nothing can travel faster than light. The paradox was that while the correlated states were instant, no actual information could be sent this way without a classical channel to compare results, thus preserving causality. It was a subtle, yet profound, distinction that would challenge the very foundations of physics.', 'advanced', now()),
(uuid_generate_v4(), 'The Architect of Venice', 'In the 16th century, the architects of Venice faced a monumental challenge: building a city on water. They devised an ingenious solution by driving thousands of wooden piles—made from alder trees, which are water-resistant—deep into the mud and sand of the lagoon. On top of these underwater forests, they laid wooden platforms and then the stone foundations of the buildings. This method was so effective that many of these ancient foundations still support the city today, a testament to their incredible foresight and engineering prowess.', 'advanced', now()),
(uuid_generate_v4(), 'Symphony of the Deep', 'Marine biologists have long been fascinated by the complex vocalizations of humpback whales. Their songs, which can last for hours and travel for hundreds of miles, are not random noises. They are structured compositions with repeating themes and phrases, akin to a symphony. Scientists hypothesize that these songs play a crucial role in social bonding and navigation. What is most remarkable is that the songs evolve over time, with new elements being incorporated across entire populations, suggesting a form of cultural transmission that is rare in the animal kingdom.', 'advanced', now());
