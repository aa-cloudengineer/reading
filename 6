import React, { useState, useEffect } from 'react';
import { Session } from '@supabase/supabase-js';
import { supabase } from './supabaseClient';
import Header from './components/Header';
import Dashboard from './components/Dashboard';
import ReadingInterface from './components/ReadingInterface';
import UztazFeedback from './components/UztazFeedback';
import Auth from './components/Auth';
import HistoryReport from './components/HistoryReport';
import { Profile, Story, UserSession, ChatMessage } from './types';
import { v4 as uuidv4 } from 'uuid';
import { LoaderCircle } from 'lucide-react';

function App() {
  const [session, setSession] = useState<Session | null>(null);
  const [loading, setLoading] = useState(true);
  const [isAnalyzing, setIsAnalyzing] = useState(false);
  const [profile, setProfile] = useState<Profile | null>(null);
  const [stories, setStories] = useState<Story[]>([]);
  const [userSessions, setUserSessions] = useState<UserSession[]>([]);
  
  const [currentView, setCurrentView] = useState<'dashboard' | 'reading' | 'history'>('dashboard');
  const [currentStory, setCurrentStory] = useState<Story | null>(null);
  const [selectedWord, setSelectedWord] = useState<string | null>(null);
  const [chatMessages, setChatMessages] = useState<ChatMessage[]>([]);

  useEffect(() => {
    const getSession = async () => {
      const { data: { session } } = await supabase.auth.getSession();
      setSession(session);
      setLoading(false);
    };
    getSession();

    const { data: { subscription } } = supabase.auth.onAuthStateChange((_event, session) => {
      setSession(session);
      if (!session) {
        setProfile(null);
        setCurrentView('dashboard');
      }
    });

    return () => subscription.unsubscribe();
  }, []);

  useEffect(() => {
    if (session?.user) {
      setLoading(true);
      const fetchData = async () => {
        try {
          // Fetch profile
          const { data: profileData, error: profileError } = await supabase
            .from('profiles')
            .select('*')
            .eq('id', session.user.id)
            .single();
          if (profileError) throw profileError;
          setProfile(profileData);

          // Fetch stories
          const { data: storiesData, error: storiesError } = await supabase
            .from('stories')
            .select('*')
            .order('created_at');
          if (storiesError) throw storiesError;
          setStories(storiesData);
          
          // Fetch all user sessions
          const { data: sessionsData, error: sessionsError } = await supabase
            .from('user_sessions')
            .select('*, stories(title)')
            .eq('user_id', session.user.id)
            .order('completed_at', { ascending: false });
          if (sessionsError) throw sessionsError;
          setUserSessions(sessionsData as UserSession[]);

          setChatMessages([
            { id: uuidv4(), type: 'assistant', content: `Welcome back, ${profileData.email}! 🦊 Ready to read?`, timestamp: new Date() }
          ]);

        } catch (error) {
          console.error('Error fetching data:', error);
          setChatMessages([{ id: uuidv4(), type: 'assistant', content: 'There was an error loading your data. Please try again.', timestamp: new Date() }]);
        } finally {
          setLoading(false);
        }
      };
      fetchData();
    } else {
      setChatMessages([
        { id: uuidv4(), type: 'assistant', content: 'Hello! I\'m Uztaz! 🦊 Sign in or create an account to start your reading adventure!', timestamp: new Date() }
      ]);
    }
  }, [session]);

  const handleStartSession = (storyId: string) => {
    const story = stories.find(s => s.id === storyId);
    if (story) {
      setCurrentStory(story);
      setCurrentView('reading');
      setChatMessages(prev => [...prev, { id: uuidv4(), type: 'assistant', content: `Great choice! Let's read "${story.title}". Click on any word if you need help.`, timestamp: new Date() }]);
    }
  };

  const handleCompleteSession = async (audioBlob: Blob | null) => {
    if (!session?.user || !currentStory || !profile || !audioBlob) return;
    
    setIsAnalyzing(true);
    setChatMessages(prev => [...prev, { id: uuidv4(), type: 'assistant', content: "Great job! Let me listen to your recording... 🧐", timestamp: new Date() }]);

    try {
      // 1. Get transcript from AssemblyAI via Supabase Edge Function
      const { data, error: functionError } = await supabase.functions.invoke('transcribe-audio', {
        body: audioBlob,
      });

      if (functionError) throw functionError;
      if (data.error) throw new Error(data.error);

      const transcript = data.transcript;

      // 2. Calculate score (simple word match percentage)
      const originalWords = currentStory.text.toLowerCase().replace(/[.,!?]/g, '').split(/\s+/).filter(Boolean);
      const transcribedWords = transcript.toLowerCase().replace(/[.,!?]/g, '').split(/\s+/).filter(Boolean);
      
      let correctCount = 0;
      const wordMap = new Map();
      originalWords.forEach(word => wordMap.set(word, (wordMap.get(word) || 0) + 1));
      transcribedWords.forEach(word => {
        if (wordMap.has(word) && wordMap.get(word) > 0) {
          correctCount++;
          wordMap.set(word, wordMap.get(word) - 1);
        }
      });
      const score = Math.round((correctCount / originalWords.length) * 100);

      // 3. Save the session
      const { data: newSessionData, error: sessionError } = await supabase.from('user_sessions').insert({
        user_id: session.user.id,
        story_id: currentStory.id,
        score: score
      }).select('*, stories(title)').single();
      if (sessionError) throw sessionError;
      
      setUserSessions(prev => [newSessionData as UserSession, ...prev]);

      // 4. Update user profile (XP, level etc.)
      const newXp = profile.xp + 50;
      const newLevel = Math.floor(newXp / 1000) + 1;

      const { data: updatedProfile, error: profileError } = await supabase
        .from('profiles')
        .update({ xp: newXp, level: newLevel })
        .eq('id', session.user.id)
        .select()
        .single();
      if (profileError) throw profileError;
      
      setProfile(updatedProfile);
      
      const congratsMessage: ChatMessage = { id: uuidv4(), type: 'assistant', content: `Wow! Based on my analysis, you read "${currentStory?.title}" with ${score}% accuracy and earned 50 XP! You're doing amazing!`, timestamp: new Date(), feedback: { corrections: [], pronunciation: [], suggestions: ['You read that so smoothly!', 'Keep up the fantastic work!', 'Ready for another story?'] } };
      setChatMessages(prev => [...prev, congratsMessage]);

    } catch(error: any) {
      console.error("Error completing session:", error);
      const errorMessage: ChatMessage = { id: uuidv4(), type: 'assistant', content: `Oops! There was an error analyzing your audio. ${error.message || 'Please try again.'}`, timestamp: new Date() };
      setChatMessages(prev => [...prev, errorMessage]);
    } finally {
      setCurrentView('dashboard');
      setIsAnalyzing(false);
    }
  };

  const handleSendMessage = (message: string) => {
    const userMessage: ChatMessage = { id: uuidv4(), type: 'user', content: message, timestamp: new Date() };
    setChatMessages(prev => [...prev, userMessage]);
    setTimeout(() => {
      let assistantResponse: ChatMessage;
      if (message.toLowerCase().includes('help me with the word')) {
        const word = message.match(/"([^"]+)"/)?.[1] || 'word';
        assistantResponse = { id: uuidv4(), type: 'assistant', content: `You got it! Let's look at the word "${word}".`, timestamp: new Date(), feedback: { corrections: [], pronunciation: [word], suggestions: [`Try saying it slowly: "${word}". Click the word to hear it!`, 'You can do it!'] } };
      } else {
        assistantResponse = { id: uuidv4(), type: 'assistant', content: 'I\'m listening! Ask me anything about pronunciation or words. I\'m here to cheer you on! 🦊', timestamp: new Date() };
      }
      setChatMessages(prev => [...prev, assistantResponse]);
    }, 1500);
  };

  const handleSignOut = async () => {
    await supabase.auth.signOut();
  };

  if ((loading || isAnalyzing) && !profile) {
    return (
      <div className="min-h-screen bg-off-white flex flex-col items-center justify-center space-y-4">
        <LoaderCircle className="w-12 h-12 text-brand animate-spin" />
        {isAnalyzing && <p className="text-gray-600 font-medium">Uztaz is analyzing your reading...</p>}
      </div>
    );
  }

  const renderContent = () => {
    switch (currentView) {
      case 'dashboard':
        return (
          <Dashboard
            profile={profile}
            stories={stories}
            userSessions={userSessions}
            onStartSession={handleStartSession}
            loading={loading}
          />
        );
      case 'history':
        return (
          <HistoryReport
            profile={profile}
            userSessions={userSessions}
          />
        );
      case 'reading':
        return currentStory ? (
          <div className="max-w-7xl mx-auto">
            <div className="grid lg:grid-cols-3 gap-8">
              <div className="lg:col-span-2">
                <ReadingInterface
                  story={currentStory}
                  onComplete={handleCompleteSession}
                  onWordSelect={setSelectedWord}
                  isAnalyzing={isAnalyzing}
                />
              </div>
              <div>
                <UztazFeedback
                  messages={chatMessages}
                  onSendMessage={handleSendMessage}
                  selectedWord={selectedWord}
                />
              </div>
            </div>
          </div>
        ) : null;
      default:
        return null;
    }
  }

  return (
    <div className="min-h-screen bg-off-white text-gray-800">
      {!session ? (
        <Auth />
      ) : (
        <>
          <Header 
            profile={profile} 
            onHomeClick={() => setCurrentView('dashboard')} 
            onHistoryClick={() => setCurrentView('history')}
            onSignOut={handleSignOut} 
          />
          <main className="py-6 sm:py-8 px-4 sm:px-6">
            {renderContent()}
          </main>
        </>
      )}
    </div>
  );
}

export default App;
