import React, { useState } from 'react';
import { motion } from 'framer-motion';
import { Mic, Square, Play, CheckCircle, Volume2, RotateCcw, LoaderCircle } from 'lucide-react';
import { useAudioRecorder } from '../hooks/useAudioRecorder';
import { Story } from '../types';

interface ReadingInterfaceProps {
  story: Story;
  onComplete: (audioBlob: Blob | null) => void;
  onWordSelect: (word: string | null) => void;
  isAnalyzing: boolean;
}

const ReadingInterface: React.FC<ReadingInterfaceProps> = ({ story, onComplete, onWordSelect, isAnalyzing }) => {
  const [selectedWord, setSelectedWord] = useState<string | null>(null);
  const [isPlaying, setIsPlaying] = useState(false);
  
  const {
    isRecording,
    recordingTime,
    audioBlob,
    startRecording,
    stopRecording,
    clearRecording,
  } = useAudioRecorder();

  const formatTime = (seconds: number): string => {
    const mins = Math.floor(seconds / 60);
    const secs = seconds % 60;
    return `${mins.toString().padStart(2, '0')}:${secs.toString().padStart(2, '0')}`;
  };

  const playRecording = () => {
    if (audioBlob) {
      const audio = new Audio(URL.createObjectURL(audioBlob));
      audio.play();
      setIsPlaying(true);
      audio.onended = () => setIsPlaying(false);
    }
  };

  const handleWordClick = (word: string) => {
    const cleanedWord = word.replace(/[.,!?]/g, '');
    setSelectedWord(cleanedWord);
    onWordSelect(cleanedWord);
  };

  const handleRecordToggle = () => {
    if (isRecording) {
      stopRecording();
    } else {
      clearRecording();
      startRecording();
    }
  };

  return (
    <div className="bg-white rounded-2xl shadow-lg p-6 md:p-8 space-y-6 h-full flex flex-col">
      <div className="flex-grow">
        <h2 className="text-3xl font-bold text-brand-dark mb-1">{story.title}</h2>
        <p className="text-gray-500 mb-6">Click any word to get help from Uztaz!</p>

        <div className="text-lg leading-relaxed text-gray-700 max-h-[50vh] overflow-y-auto pr-2">
          {story.text.split(/(\s+)/).map((segment, index) => (
            <React.Fragment key={index}>
              {segment.match(/\s+/) ? (
                <span>{segment}</span>
              ) : (
                <motion.span
                  className={`cursor-pointer hover:bg-brand-light px-1 py-0.5 rounded-md transition-colors ${
                    selectedWord === segment.replace(/[.,!?]/g, '') ? 'bg-yellow-200' : ''
                  }`}
                  onClick={() => handleWordClick(segment)}
                  whileHover={{ scale: 1.05 }}
                  whileTap={{ scale: 0.95 }}
                >
                  {segment}
                </motion.span>
              )}
            </React.Fragment>
          ))}
        </div>
      </div>

      <div className="bg-gray-50 rounded-xl p-4 mt-auto">
        {isRecording && (
          <motion.div 
            className="flex items-center justify-center space-x-2 text-danger mb-3"
            initial={{ opacity: 0 }}
            animate={{ opacity: 1 }}
          >
            <motion.div 
              className="w-3 h-3 bg-danger rounded-full"
              animate={{ scale: [1, 1.2, 1] }}
              transition={{ repeat: Infinity, duration: 1 }}
            />
            <span className="font-medium text-sm">Recording... {formatTime(recordingTime)}</span>
          </motion.div>
        )}

        {audioBlob && !isRecording && (
          <motion.div 
            className="bg-brand-light rounded-lg p-3 mb-3"
            initial={{ opacity: 0, y: 10 }}
            animate={{ opacity: 1, y: 0 }}
          >
            <div className="flex items-center justify-between">
              <div className="flex items-center space-x-3">
                <motion.button
                  onClick={playRecording}
                  className="bg-brand text-white p-2 rounded-full shadow-sm"
                  whileHover={{ scale: 1.1 }}
                  disabled={isPlaying}
                >
                  <Volume2 className="h-5 w-5" />
                </motion.button>
                <span className="text-sm text-brand-dark font-medium">
                  {isPlaying ? 'Playing...' : 'Listen to your recording'}
                </span>
              </div>
              <motion.button
                onClick={clearRecording}
                className="text-gray-500 hover:text-danger"
                whileHover={{ scale: 1.1, rotate: 45 }}
              >
                <RotateCcw className="h-5 w-5" />
              </motion.button>
            </div>
          </motion.div>
        )}
        
        <div className="grid grid-cols-1 md:grid-cols-2 gap-4">
          <motion.button
            onClick={handleRecordToggle}
            className={`flex items-center justify-center space-x-2 py-3 px-4 rounded-lg font-semibold transition-all shadow-md ${
              isRecording 
              ? 'bg-danger text-white' 
              : 'bg-white border border-gray-300 text-gray-700 hover:bg-gray-100'
            }`}
            whileHover={{ scale: 1.02 }}
            whileTap={{ scale: 0.98 }}
            disabled={isAnalyzing}
          >
            {isRecording ? <Square className="h-5 w-5" /> : <Mic className="h-5 w-5" />}
            <span>{isRecording ? 'Stop Recording' : (audioBlob ? 'Record Again' : 'Record')}</span>
          </motion.button>

          <motion.button
            onClick={() => onComplete(audioBlob)}
            className="flex items-center justify-center space-x-2 py-3 px-4 rounded-lg font-semibold transition-all shadow-md bg-success text-white disabled:bg-gray-300 disabled:cursor-not-allowed"
            whileHover={{ scale: 1.02 }}
            whileTap={{ scale: 0.98 }}
            disabled={!audioBlob || isRecording || isAnalyzing}
          >
            {isAnalyzing ? <LoaderCircle className="h-5 w-5 animate-spin" /> : <CheckCircle className="h-5 w-5" />}
            <span>{isAnalyzing ? 'Analyzing...' : 'Finish & Get Feedback'}</span>
          </motion.button>
        </div>
      </div>
    </div>
  );
};

export default ReadingInterface;
