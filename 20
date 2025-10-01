import React from 'react';
import { motion } from 'framer-motion';
import { Play, Clock, TrendingUp, Award, BookOpen, LoaderCircle } from 'lucide-react';
import { Story, Profile, UserSession } from '../types';

interface DashboardProps {
  profile: Profile | null;
  stories: Story[];
  userSessions: UserSession[];
  onStartSession: (storyId: string) => void;
  loading: boolean;
}

const Dashboard: React.FC<DashboardProps> = ({ 
  profile, 
  stories,
  userSessions,
  onStartSession,
  loading
}) => {
  const getDifficultyStyles = (difficulty: string) => {
    switch (difficulty) {
      case 'beginner': return { tag: 'bg-success/20 text-green-700', border: 'border-success' };
      case 'intermediate': return { tag: 'bg-warning/20 text-yellow-700', border: 'border-warning' };
      case 'advanced': return { tag: 'bg-danger/20 text-red-700', border: 'border-danger' };
      default: return { tag: 'bg-gray-200 text-gray-800', border: 'border-gray-300' };
    }
  };

  const recentSessions = userSessions.slice(0, 5);
  const averageScore = recentSessions.length > 0
    ? Math.round(recentSessions.reduce((acc, s) => acc + s.score, 0) / recentSessions.length)
    : 0;

  const stats = [
    { label: 'Avg. Score (Recent)', value: `${averageScore}%`, icon: TrendingUp, color: 'text-success' },
    { label: 'Stories Read', value: userSessions.length, icon: BookOpen, color: 'text-accent' },
    { label: 'Current Level', value: profile?.level || 1, icon: Award, color: 'text-brand' },
    { label: 'Reading Streak', value: `${profile?.streak_days || 0} days`, icon: Clock, color: 'text-primary' },
  ];

  return (
    <div className="max-w-7xl mx-auto space-y-10 sm:space-y-12">
      <motion.div
        initial={{ opacity: 0, y: 20 }}
        animate={{ opacity: 1, y: 0 }}
        className="text-center"
      >
        <h2 className="text-3xl sm:text-4xl font-bold text-brand-dark mb-2">
          Ready for a reading adventure?
        </h2>
        <p className="text-gray-600 text-base sm:text-lg">
          Uztaz is cheering for you! Pick a story and let's begin.
        </p>
      </motion.div>

      <motion.div 
        className="grid grid-cols-2 md:grid-cols-4 gap-4 sm:gap-6"
        initial={{ opacity: 0 }}
        animate={{ opacity: 1 }}
        transition={{ staggerChildren: 0.1, delayChildren: 0.2 }}
      >
        {stats.map((stat) => (
          <motion.div
            key={stat.label}
            className="bg-white rounded-2xl shadow-md p-4 sm:p-6 text-center transition-transform duration-300 hover:-translate-y-2"
            variants={{ hidden: { y: 20, opacity: 0 }, visible: { y: 0, opacity: 1 } }}
          >
            <stat.icon className={`h-8 w-8 sm:h-10 sm:w-10 mx-auto mb-3 ${stat.color}`} strokeWidth={1.5} />
            <div className="text-2xl sm:text-3xl font-bold text-gray-800">{stat.value}</div>
            <div className="text-xs sm:text-sm text-gray-500 mt-1">{stat.label}</div>
          </motion.div>
        ))}
      </motion.div>

      <motion.div
        initial={{ opacity: 0, y: 20 }}
        animate={{ opacity: 1, y: 0 }}
        transition={{ delay: 0.4 }}
      >
        <h3 className="text-2xl sm:text-3xl font-bold text-brand-dark mb-6">Pick a Story to Read</h3>
        {loading ? (
          <div className="flex justify-center items-center h-64">
            <LoaderCircle className="w-10 h-10 text-brand animate-spin" />
          </div>
        ) : (
          <div className="grid md:grid-cols-2 lg:grid-cols-3 gap-6 sm:gap-8">
            {stories.map((story, index) => {
              const styles = getDifficultyStyles(story.difficulty);
              return (
              <motion.div
                key={story.id}
                className={`bg-white rounded-2xl shadow-lg overflow-hidden flex flex-col border-b-4 ${styles.border}`}
                whileHover={{ scale: 1.03, boxShadow: 'var(--tw-shadow-xl)' }}
                transition={{ type: 'spring', stiffness: 300 }}
                initial={{ opacity: 0, y: 20 }}
                animate={{ opacity: 1, y: 0 }}
                transition={{ delay: 0.1 * index }}
              >
                <div className="p-6 flex-grow">
                  <span className={`px-3 py-1 text-xs font-bold uppercase rounded-full ${styles.tag}`}>
                    {story.difficulty}
                  </span>
                  <h4 className="text-xl font-semibold text-gray-800 my-3">
                    {story.title}
                  </h4>
                  <p className="text-gray-600 text-sm mb-4 line-clamp-3">
                    {story.text}
                  </p>
                </div>
                
                <div className="p-6 bg-gray-50 mt-auto">
                  <motion.button
                    onClick={() => onStartSession(story.id)}
                    className="w-full bg-brand hover:bg-brand-dark text-white py-3 px-4 rounded-lg font-semibold shadow-md hover:shadow-lg transition-all flex items-center justify-center space-x-2"
                    whileHover={{ y: -2 }}
                    whileTap={{ scale: 0.98 }}
                  >
                    <Play className="h-5 w-5" />
                    <span>Start Reading</span>
                  </motion.button>
                </div>
              </motion.div>
            )})}
          </div>
        )}
      </motion.div>
    </div>
  );
};

export default Dashboard;
