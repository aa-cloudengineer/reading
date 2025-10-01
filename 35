import React from 'react';
import { motion } from 'framer-motion';
import { format } from 'date-fns';
import { TrendingUp, Star, BookCheck, History } from 'lucide-react';
import { Profile, UserSession } from '../types';

interface HistoryReportProps {
  profile: Profile | null;
  userSessions: UserSession[];
}

const HistoryReport: React.FC<HistoryReportProps> = ({ profile, userSessions }) => {
  const totalSessions = userSessions.length;
  const averageScore = totalSessions > 0
    ? Math.round(userSessions.reduce((acc, s) => acc + s.score, 0) / totalSessions)
    : 0;
  const bestScore = totalSessions > 0
    ? Math.max(...userSessions.map(s => s.score))
    : 0;

  const stats = [
    { label: 'Total Stories Read', value: totalSessions, icon: BookCheck, color: 'text-accent' },
    { label: 'Average Score', value: `${averageScore}%`, icon: TrendingUp, color: 'text-success' },
    { label: 'Best Score', value: `${bestScore}%`, icon: Star, color: 'text-yellow-500' },
  ];

  const getDifficultyColor = (score: number) => {
    if (score >= 90) return 'text-success';
    if (score >= 70) return 'text-yellow-500';
    return 'text-danger';
  };

  return (
    <div className="max-w-7xl mx-auto space-y-10 sm:space-y-12">
      <motion.div
        initial={{ opacity: 0, y: 20 }}
        animate={{ opacity: 1, y: 0 }}
        className="text-center"
      >
        <h2 className="text-3xl sm:text-4xl font-bold text-brand-dark mb-2">
          Your Reading Journey
        </h2>
        <p className="text-gray-600 text-base sm:text-lg">
          Track your progress and see how much you've improved!
        </p>
      </motion.div>

      <motion.div 
        className="grid grid-cols-1 sm:grid-cols-3 gap-4 sm:gap-6"
        initial={{ opacity: 0 }}
        animate={{ opacity: 1 }}
        transition={{ staggerChildren: 0.1, delayChildren: 0.2 }}
      >
        {stats.map((stat) => (
          <motion.div
            key={stat.label}
            className="bg-white rounded-2xl shadow-md p-6 text-center"
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
        <h3 className="text-2xl sm:text-3xl font-bold text-brand-dark mb-6 flex items-center gap-2">
          <History />
          Session History
        </h3>
        <div className="bg-white rounded-2xl shadow-lg overflow-hidden">
          <div className="overflow-x-auto">
            <table className="w-full text-sm text-left text-gray-600">
              <thead className="text-xs text-gray-700 uppercase bg-gray-50">
                <tr>
                  <th scope="col" className="px-6 py-3">Story Title</th>
                  <th scope="col" className="px-6 py-3">Score</th>
                  <th scope="col" className="px-6 py-3">Date</th>
                </tr>
              </thead>
              <tbody>
                {userSessions.length > 0 ? (
                  userSessions.map((session, index) => (
                    <motion.tr 
                      key={session.id}
                      className="bg-white border-b hover:bg-gray-50"
                      initial={{ opacity: 0, y: 10 }}
                      animate={{ opacity: 1, y: 0 }}
                      transition={{ delay: 0.05 * index }}
                    >
                      <th scope="row" className="px-6 py-4 font-medium text-gray-900 whitespace-nowrap">
                        {session.stories?.title || 'A story'}
                      </th>
                      <td className={`px-6 py-4 font-bold ${getDifficultyColor(session.score)}`}>
                        {session.score}%
                      </td>
                      <td className="px-6 py-4">
                        {format(new Date(session.completed_at), 'MMMM d, yyyy')}
                      </td>
                    </motion.tr>
                  ))
                ) : (
                  <tr>
                    <td colSpan={3} className="text-center py-10 text-gray-500">
                      You haven't completed any stories yet. Let's get reading!
                    </td>
                  </tr>
                )}
              </tbody>
            </table>
          </div>
        </div>
      </motion.div>
    </div>
  );
};

export default HistoryReport;
