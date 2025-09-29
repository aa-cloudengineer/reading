// Represents a story fetched from the database
export interface Story {
  id: string;
  title: string;
  text: string;
  difficulty: 'beginner' | 'intermediate' | 'advanced';
  created_at: string;
}

// Represents a user's profile from the database
export interface Profile {
  id: string; // Corresponds to auth.users.id
  email: string;
  xp: number;
  level: number;
  streak_days: number;
  updated_at: string;
}

// Represents a completed reading session by a user
export interface UserSession {
  id: number;
  user_id: string;
  story_id: string;
  score: number;
  completed_at: string;
  // We can join to get story details if needed
  stories?: Story; 
}

// Represents a recording a user makes (for future use)
export interface Recording {
  id: string;
  audioBlob: Blob;
  timestamp: Date;
  duration: number;
}

// Represents a message in the chat interface
export interface ChatMessage {
  id:string;
  type: 'user' | 'assistant';
  content: string;
  timestamp: Date;
  feedback?: {
    corrections: string[];
    pronunciation: string[];
    suggestions: string[];
  };
}
