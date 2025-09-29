import { useCallback } from 'react';

interface SpeakOptions {
  text: string;
  lang?: string;
  rate?: number;
}

export const useTextToSpeech = () => {
  const speak = useCallback((options: SpeakOptions) => {
    if (!window.speechSynthesis) {
      console.warn('Text-to-speech not supported in this browser.');
      return;
    }
    
    const { text, lang = 'en-US', rate = 1 } = options;
    const utterance = new SpeechSynthesisUtterance(text);
    
    utterance.lang = lang;
    utterance.rate = rate;
    
    // Cancel any previous speech to prevent overlap
    window.speechSynthesis.cancel();
    window.speechSynthesis.speak(utterance);
  }, []);

  return { speak };
};
