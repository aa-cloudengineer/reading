import React, { useState, useRef, useEffect } from 'react';
import { motion, AnimatePresence } from 'framer-motion';
import { Send, User, Lightbulb, Volume2, CheckCircle } from 'lucide-react';
import { ChatMessage } from '../types';
import FoxMascot from './FoxMascot';
import { useTextToSpeech } from '../hooks/useTextToSpeech';

interface UztazFeedbackProps {
  messages: ChatMessage[];
  onSendMessage: (message: string) => void;
  selectedWord?: string | null;
}

const UztazFeedback: React.FC<UztazFeedbackProps> = ({ 
  messages, 
  onSendMessage, 
  selectedWord 
}) => {
  const [inputValue, setInputValue] = useState('');
  const [isTyping, setIsTyping] = useState(false);
  const messagesEndRef = useRef<HTMLDivElement>(null);
  const { speak } = useTextToSpeech();

  useEffect(() => {
    messagesEndRef.current?.scrollIntoView({ behavior: 'smooth' });
  }, [messages]);

  useEffect(() => {
    if (selectedWord) {
      handleWordHelp(selectedWord);
    }
  }, [selectedWord]);

  const handleSubmit = (e: React.FormEvent) => {
    e.preventDefault();
    if (inputValue.trim()) {
      onSendMessage(inputValue.trim());
      setInputValue('');
      simulateTyping();
    }
  };

  const handleWordHelp = (word: string) => {
    onSendMessage(`Help me with the word: "${word}"`);
    simulateTyping();
  };

  const simulateTyping = () => {
    setIsTyping(true);
    setTimeout(() => setIsTyping(false), 1500);
  };

  const playPronunciation = (word: string) => {
    speak({ text: word, lang: 'en-US', rate: 0.8 });
  };

  return (
    <div className="bg-white rounded-2xl shadow-lg h-full flex flex-col">
      <div className="p-4 border-b border-gray-200 text-center">
        <div className="w-20 h-20 mx-auto">
          <FoxMascot />
        </div>
        <h3 className="font-bold text-lg text-brand-dark mt-2">Uztaz's Feedback 🦊</h3>
      </div>

      <div className="flex-1 overflow-y-auto p-4 space-y-4">
        <AnimatePresence initial={false}>
          {messages.map((message) => (
            <motion.div
              key={message.id}
              layout
              initial={{ opacity: 0, scale: 0.8, y: 50 }}
              animate={{ opacity: 1, scale: 1, y: 0 }}
              exit={{ opacity: 0, scale: 0.5, transition: { duration: 0.2 } }}
              className={`flex items-end gap-2 ${message.type === 'user' ? 'justify-end' : 'justify-start'}`}
            >
              {message.type === 'assistant' && (
                <div className="w-8 h-8 rounded-full bg-accent flex-shrink-0 self-start">
                  <FoxMascot />
                </div>
              )}
              <div
                className={`max-w-xs lg:max-w-sm px-4 py-3 rounded-2xl ${
                  message.type === 'user'
                    ? 'bg-primary text-white rounded-br-lg'
                    : 'bg-brand-light text-gray-800 rounded-bl-lg'
                }`}
              >
                <p className="text-sm">{message.content}</p>
                {message.feedback && (
                  <div className="mt-3 space-y-2">
                    {message.feedback.pronunciation.length > 0 && (
                      <div className="bg-white/70 rounded-lg p-2">
                        <h4 className="text-xs font-bold text-brand-dark mb-1">Pronunciation Helper</h4>
                        {message.feedback.pronunciation.map((word, idx) => (
                          <motion.button
                            key={idx}
                            onClick={() => playPronunciation(word)}
                            className="text-sm font-semibold text-brand hover:text-brand-dark flex items-center gap-1"
                            whileHover={{ scale: 1.05 }}
                          >
                            <Volume2 className="h-4 w-4" />
                            Listen to "{word}"
                          </motion.button>
                        ))}
                      </div>
                    )}
                    {message.feedback.suggestions.length > 0 && (
                      <div className="bg-white/70 rounded-lg p-2 mt-2">
                        <h4 className="text-xs font-bold text-green-700 mb-1">Uztaz's Tips!</h4>
                        {message.feedback.suggestions.map((suggestion, idx) => (
                          <p key={idx} className="text-xs text-green-800 flex items-start gap-1">
                            <CheckCircle className="h-3 w-3 mt-0.5 flex-shrink-0" />
                            <span>{suggestion}</span>
                          </p>
                        ))}
                      </div>
                    )}
                  </div>
                )}
              </div>
              {message.type === 'user' && (
                <User className="h-6 w-6 p-1 rounded-full bg-gray-200 text-gray-600 flex-shrink-0" />
              )}
            </motion.div>
          ))}
        </AnimatePresence>

        {isTyping && (
          <motion.div
            initial={{ opacity: 0, y: 10 }} animate={{ opacity: 1, y: 0 }}
            className="flex items-end gap-2"
          >
            <div className="w-8 h-8 rounded-full bg-accent"><FoxMascot /></div>
            <div className="bg-brand-light px-4 py-3 rounded-2xl rounded-bl-lg">
              <div className="flex items-center space-x-1">
                <div className="w-2 h-2 bg-gray-400 rounded-full animate-bounce"></div>
                <div className="w-2 h-2 bg-gray-400 rounded-full animate-bounce" style={{animationDelay: '0.1s'}}></div>
                <div className="w-2 h-2 bg-gray-400 rounded-full animate-bounce" style={{animationDelay: '0.2s'}}></div>
              </div>
            </div>
          </motion.div>
        )}
        <div ref={messagesEndRef} />
      </div>

      <form onSubmit={handleSubmit} className="p-4 border-t border-gray-200 bg-gray-50 rounded-b-2xl">
        <div className="flex space-x-2">
          <input
            type="text"
            value={inputValue}
            onChange={(e) => setInputValue(e.target.value)}
            placeholder="Ask Uztaz for help..."
            className="flex-1 px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-brand text-sm"
          />
          <motion.button
            type="submit"
            className="bg-brand text-white p-2.5 rounded-lg shadow-md disabled:bg-gray-300"
            whileHover={{ scale: 1.05 }}
            whileTap={{ scale: 0.95 }}
            disabled={!inputValue.trim()}
          >
            <Send className="h-5 w-5" />
          </motion.button>
        </div>
      </form>
    </div>
  );
};

export default UztazFeedback;
