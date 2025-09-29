import React, { useState } from 'react';
import { motion } from 'framer-motion';
import { AtSign, Lock, LogIn, UserPlus, LoaderCircle } from 'lucide-react';
import { supabase } from '../supabaseClient';
import FoxMascot from './FoxMascot';

const Auth: React.FC = () => {
  const [isLogin, setIsLogin] = useState(true);
  const [email, setEmail] = useState('');
  const [password, setPassword] = useState('');
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState<string | null>(null);
  const [message, setMessage] = useState<string | null>(null);

  const handleAuthAction = async (e: React.FormEvent) => {
    e.preventDefault();
    setLoading(true);
    setError(null);
    setMessage(null);

    try {
      let response;
      if (isLogin) {
        response = await supabase.auth.signInWithPassword({ email, password });
      } else {
        response = await supabase.auth.signUp({ email, password });
        if (response.data.user && response.data.user.identities?.length === 0) {
           setMessage("Signup successful! Please check your email to verify your account.");
        }
      }
      
      const { error } = response;
      if (error) throw error;

    } catch (error: any) {
      setError(error.error_description || error.message);
    } finally {
      setLoading(false);
    }
  };

  const handleGoogleSignIn = async () => {
    setLoading(true);
    setError(null);
    const { error } = await supabase.auth.signInWithOAuth({
      provider: 'google',
      options: {
        redirectTo: window.location.origin,
      },
    });
    if (error) {
      setError(error.message);
      setLoading(false);
    }
  };

  return (
    <div className="min-h-screen flex items-center justify-center bg-brand-light p-4">
      <motion.div 
        className="w-full max-w-md bg-white rounded-2xl shadow-xl p-8 space-y-6"
        initial={{ opacity: 0, y: -50 }}
        animate={{ opacity: 1, y: 0 }}
        transition={{ type: 'spring', stiffness: 100 }}
      >
        <div className="text-center">
          <div className="w-24 h-24 mx-auto mb-4">
            <FoxMascot />
          </div>
          <h1 className="text-3xl font-bold text-brand-dark">Welcome to EchoRead</h1>
          <p className="text-gray-500 mt-2">{isLogin ? 'Sign in to continue your adventure!' : 'Create an account to get started!'}</p>
        </div>

        {error && <div className="bg-danger/10 text-danger p-3 rounded-lg text-sm text-center">{error}</div>}
        {message && <div className="bg-success/10 text-green-700 p-3 rounded-lg text-sm text-center">{message}</div>}

        <form onSubmit={handleAuthAction} className="space-y-4">
          <div className="relative">
            <AtSign className="absolute left-3 top-1/2 -translate-y-1/2 h-5 w-5 text-gray-400" />
            <input
              type="email"
              placeholder="Email address"
              value={email}
              onChange={(e) => setEmail(e.target.value)}
              required
              className="w-full pl-10 pr-3 py-2.5 border border-gray-300 rounded-lg focus:ring-2 focus:ring-brand focus:outline-none transition"
            />
          </div>
          <div className="relative">
            <Lock className="absolute left-3 top-1/2 -translate-y-1/2 h-5 w-5 text-gray-400" />
            <input
              type="password"
              placeholder="Password"
              value={password}
              onChange={(e) => setPassword(e.target.value)}
              required
              className="w-full pl-10 pr-3 py-2.5 border border-gray-300 rounded-lg focus:ring-2 focus:ring-brand focus:outline-none transition"
            />
          </div>
          <motion.button
            type="submit"
            disabled={loading}
            className="w-full bg-brand hover:bg-brand-dark text-white font-bold py-3 px-4 rounded-lg transition-all shadow-md flex items-center justify-center space-x-2"
            whileHover={{ scale: 1.02 }}
            whileTap={{ scale: 0.98 }}
          >
            {loading ? <LoaderCircle className="animate-spin" /> : (isLogin ? <LogIn /> : <UserPlus />)}
            <span>{loading ? 'Processing...' : (isLogin ? 'Sign In' : 'Create Account')}</span>
          </motion.button>
        </form>

        <div className="relative flex items-center">
          <div className="flex-grow border-t border-gray-300"></div>
          <span className="flex-shrink mx-4 text-gray-500 text-sm">OR</span>
          <div className="flex-grow border-t border-gray-300"></div>
        </div>

        <motion.button
          onClick={handleGoogleSignIn}
          disabled={loading}
          className="w-full bg-white border border-gray-300 text-gray-700 font-semibold py-3 px-4 rounded-lg transition-all shadow-sm hover:bg-gray-50 flex items-center justify-center space-x-2"
          whileHover={{ scale: 1.02 }}
          whileTap={{ scale: 0.98 }}
        >
          <svg className="w-5 h-5" viewBox="0 0 48 48">
            <path fill="#FFC107" d="M43.611,20.083H42V20H24v8h11.303c-1.649,4.657-6.08,8-11.303,8c-6.627,0-12-5.373-12-12s5.373-12,12-12c3.059,0,5.842,1.154,7.961,3.039l5.657-5.657C34.046,6.053,29.268,4,24,4C12.955,4,4,12.955,4,24s8.955,20,20,20s20-8.955,20-20C44,22.659,43.862,21.35,43.611,20.083z"></path><path fill="#FF3D00" d="M6.306,14.691l6.571,4.819C14.655,15.108,18.961,12,24,12c3.059,0,5.842,1.154,7.961,3.039l5.657-5.657C34.046,6.053,29.268,4,24,4C16.318,4,9.656,8.337,6.306,14.691z"></path><path fill="#4CAF50" d="M24,44c5.166,0,9.86-1.977,13.409-5.192l-6.19-5.238C29.211,35.091,26.715,36,24,36c-5.202,0-9.619-3.317-11.283-7.946l-6.522,5.025C9.505,39.556,16.227,44,24,44z"></path><path fill="#1976D2" d="M43.611,20.083H42V20H24v8h11.303c-0.792,2.237-2.231,4.166-4.087,5.571l6.19,5.238C41.386,36.258,44,30.638,44,24C44,22.659,43.862,21.35,43.611,20.083z"></path>
          </svg>
          <span>Sign in with Google</span>
        </motion.button>

        <div className="text-center text-sm">
          <p>
            {isLogin ? "Don't have an account?" : "Already have an account?"}
            <button
              onClick={() => {
                setIsLogin(!isLogin);
                setError(null);
                setMessage(null);
              }}
              className="font-semibold text-brand hover:underline ml-1"
            >
              {isLogin ? 'Sign Up' : 'Sign In'}
            </button>
          </p>
        </div>
      </motion.div>
    </div>
  );
};

export default Auth;
