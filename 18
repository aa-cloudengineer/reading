import React from 'react';
import { motion } from 'framer-motion';
import { Trophy, Flame, LogOut, Home, History } from 'lucide-react';
import { Profile } from '../types';

interface HeaderProps {
  profile: Profile | null;
  onHomeClick: () => void;
  onHistoryClick: () => void;
  onSignOut: () => void;
}

const Header: React.FC<HeaderProps> = ({ profile, onHomeClick, onHistoryClick, onSignOut }) => {
  const progressPercentage = profile ? (profile.xp % 1000) / 10 : 0;

  return (
    <motion.header 
      className="bg-white text-gray-800 p-3 sm:p-4 shadow-sm sticky top-0 z-50"
      initial={{ y: -100 }}
      animate={{ y: 0 }}
      transition={{ type: "spring", stiffness: 100 }}
    >
      <div className="max-w-7xl mx-auto flex items-center justify-between">
        <div 
          className="flex items-center space-x-2 sm:space-x-3 cursor-pointer"
          onClick={onHomeClick}
        >
          <motion.div whileHover={{ rotate: 10 }}>
            <span className="text-3xl">🦊</span>
          </motion.div>
          <div>
            <h1 className="text-xl sm:text-2xl font-bold text-brand-dark">EchoRead</h1>
            <p className="hidden sm:block text-xs text-gray-500">“Read it, hear it, nail it!”</p>
          </div>
        </div>

        {profile && (
          <div className="flex items-center space-x-2 sm:space-x-4">
            <div className="flex items-center space-x-2 sm:space-x-4 bg-brand-light p-2 rounded-full">
              {/* Navigation */}
              <button onClick={onHomeClick} className="flex items-center space-x-1 text-gray-600 hover:text-brand px-2 py-1 rounded-md transition-colors">
                <Home className="h-4 w-4 sm:h-5 sm:w-5" />
                <span className="hidden md:inline text-sm font-medium">Dashboard</span>
              </button>
              <button onClick={onHistoryClick} className="flex items-center space-x-1 text-gray-600 hover:text-brand px-2 py-1 rounded-md transition-colors">
                <History className="h-4 w-4 sm:h-5 sm:w-5" />
                <span className="hidden md:inline text-sm font-medium">History</span>
              </button>
            </div>

            <div className="hidden sm:flex items-center space-x-4 md:space-x-6 bg-brand-light p-2 rounded-full">
              {/* Level & XP */}
              <div className="flex items-center space-x-2">
                <Trophy className="h-5 w-5 text-brand-dark" />
                <div className="text-left">
                  <span className="font-semibold text-sm">Level {profile.level}</span>
                  <div className="w-20 bg-gray-300 rounded-full h-1.5 mt-0.5">
                    <motion.div 
                      className="bg-gradient-to-r from-accent to-secondary h-1.5 rounded-full"
                      initial={{ width: 0 }}
                      animate={{ width: `${progressPercentage}%` }}
                      transition={{ duration: 1, ease: "easeOut" }}
                    />
                  </div>
                </div>
              </div>

              {/* Streak */}
              <div className="flex items-center space-x-2">
                <Flame className="h-5 w-5 text-accent" />
                <div className="text-left">
                  <span className="font-semibold text-sm">{profile.streak_days}</span>
                  <p className="text-xs text-gray-600 hidden lg:block">day streak</p>
                </div>
              </div>
            </div>
            
            <motion.button
              onClick={onSignOut}
              className="bg-gray-200 hover:bg-gray-300 text-gray-600 p-2.5 rounded-full transition-colors"
              whileHover={{ scale: 1.1 }}
              whileTap={{ scale: 0.9 }}
              title="Sign Out"
            >
              <LogOut className="h-5 w-5" />
            </motion.button>
          </div>
        )}
      </div>
    </motion.header>
  );
};

export default Header;
