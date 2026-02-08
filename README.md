import React, { useState, useEffect } from 'react';
import { motion, AnimatePresence } from 'framer-motion';
import { Heart, Stars, Calendar, MapPin, MessageCircle, Gift, Coffee } from 'lucide-react';

const ValentineWebsite = () => {
  const [isOpen, setIsOpen] = useState(false);
  const [hugsSent, setHugsSent] = useState(0);
  const [showSurprise, setShowSurprise] = useState(false);

  // Fixed count as requested: 432 days
  // We calculate the hours, minutes, and seconds dynamically for a "live" feel
  const [timeElapsed, setTimeElapsed] = useState({
    days: 432, hours: 0, minutes: 0, seconds: 0
  });

  useEffect(() => {
    const timer = setInterval(() => {
      const now = new Date();
      setTimeElapsed(prev => ({
        ...prev,
        hours: now.getHours(),
        minutes: now.getMinutes(),
        seconds: now.getSeconds()
      }));
    }, 1000);
    return () => clearInterval(timer);
  }, []);

  // Background Plushie Components (SVG based)
  const Plushie = ({ delay, x, y, type }) => (
    <motion.div
      initial={{ y: 20, opacity: 0 }}
      animate={{ 
        y: [0, -20, 0],
        rotate: [0, 5, -5, 0],
        opacity: 0.4
      }}
      transition={{ 
        duration: 4, 
        repeat: Infinity, 
        delay: delay,
        ease: "easeInOut" 
      }}
      className="absolute pointer-events-none"
      style={{ left: x, top: y }}
    >
      {type === 'bear' ? (
        <svg width="60" height="60" viewBox="0 0 24 24" fill="#FFB7C5" xmlns="http://www.w3.org/2000/svg">
          <circle cx="7" cy="7" r="3" />
          <circle cx="17" cy="7" r="3" />
          <rect x="5" y="8" width="14" height="12" rx="6" />
          <circle cx="9" cy="12" r="1" fill="white" />
          <circle cx="15" cy="12" r="1" fill="white" />
          <path d="M11 15C11 15 11.5 16 12 16C12.5 16 13 15 13 15" stroke="white" strokeWidth="0.5" />
        </svg>
      ) : (
        <svg width="50" height="50" viewBox="0 0 24 24" fill="#D1C4E9" xmlns="http://www.w3.org/2000/svg">
          <ellipse cx="8" cy="6" rx="2" ry="5" />
          <ellipse cx="16" cy="6" rx="2" ry="5" />
          <circle cx="12" cy="14" r="7" />
          <circle cx="10" cy="12" r="0.8" fill="white" />
          <circle cx="14" cy="12" r="0.8" fill="white" />
        </svg>
      )}
    </motion.div>
  );

  return (
    <div className="min-h-screen bg-[#FFF5F7] font-sans text-slate-800 overflow-x-hidden relative">
      {/* Background Elements */}
      <div className="fixed inset-0 z-0 overflow-hidden">
        <Plushie type="bear" x="10%" y="15%" delay={0} />
        <Plushie type="bunny" x="85%" y="20%" delay={1} />
        <Plushie type="bear" x="75%" y="70%" delay={2} />
        <Plushie type="bunny" x="15%" y="80%" delay={1.5} />
        
        {/* Floating Hearts */}
        {[...Array(12)].map((_, i) => (
          <motion.div
            key={i}
            className="absolute text-pink-200"
            initial={{ y: '100vh', x: Math.random() * 100 + 'vw' }}
            animate={{ y: '-10vh' }}
            transition={{ 
              duration: 10 + Math.random() * 10, 
              repeat: Infinity, 
              delay: Math.random() * 10 
            }}
          >
            <Heart size={16 + Math.random() * 24} fill="currentColor" />
          </motion.div>
        ))}
      </div>

      {/* Main Content */}
      <main className="relative z-10 max-w-2xl mx-auto px-6 py-12 flex flex-col items-center">
        
        {/* Hero Section */}
        <motion.div 
          initial={{ opacity: 0, y: 20 }}
          animate={{ opacity: 1, y: 0 }}
          className="text-center mb-16"
        >
          <motion.div
            animate={{ scale: [1, 1.1, 1] }}
            transition={{ duration: 2, repeat: Infinity }}
            className="inline-block mb-4"
          >
            <Heart size={64} className="text-rose-500" fill="#F43F5E" />
          </motion.div>
          <h1 className="text-4xl md:text-5xl font-serif font-bold text-rose-600 mb-2">
            Happy Valentine's Day
          </h1>
          <p className="text-rose-400 italic text-lg">Distance means so little when someone means so much.</p>
        </motion.div>

        {/* Love Timer - Updated to 432 Days */}
        <motion.div 
          initial={{ opacity: 0 }}
          whileInView={{ opacity: 1 }}
          className="w-full bg-white/60 backdrop-blur-md rounded-3xl p-8 shadow-sm border border-rose-100 mb-12 text-center"
        >
          <h2 className="text-sm uppercase tracking-widest text-rose-400 font-bold mb-6 flex items-center justify-center gap-2">
            <Calendar size={16} /> Our Love Story
          </h2>
          <div className="grid grid-cols-4 gap-4">
            {Object.entries(timeElapsed).map(([unit, value]) => (
              <div key={unit} className="flex flex-col">
                <span className="text-3xl font-bold text-rose-500">
                    {value.toString().padStart(2, '0')}
                </span>
                <span className="text-xs text-slate-500 capitalize">{unit}</span>
              </div>
            ))}
          </div>
          <p className="mt-6 text-slate-600 text-sm">...432 beautiful days of loving you.</p>
        </motion.div>

        {/* Interactive Letter */}
        <div className="w-full mb-12">
          <motion.button
            whileHover={{ scale: 1.02 }}
            whileTap={{ scale: 0.98 }}
            onClick={() => setIsOpen(!isOpen)}
            className="w-full bg-rose-500 text-white py-4 rounded-2xl font-bold shadow-lg shadow-rose-200 flex items-center justify-center gap-2"
          >
            <MessageCircle size={20} />
            {isOpen ? "Close My Letter" : "Open My Love Letter"}
          </motion.button>

          <AnimatePresence>
            {isOpen && (
              <motion.div
                initial={{ height: 0, opacity: 0 }}
                animate={{ height: 'auto', opacity: 1 }}
                exit={{ height: 0, opacity: 0 }}
                className="overflow-hidden"
              >
                <div className="mt-4 bg-white p-8 rounded-2xl shadow-inner border-l-4 border-rose-300 italic text-slate-700 leading-relaxed">
                  "My Dearest,<br /><br />
                  Even though there are miles between us right now, my heart has never felt closer to yours. Every morning I wake up grateful for the technology that lets me see your smile, and every night I fall asleep counting down the days until we don't have to say goodbye through a screen anymore.<br /><br />
                  You are my favorite person, my best friend, and my home. Distance is just a test to see how far love can travel, and I think we're winning.<br /><br />
                  Forever yours."
                </div>
              </motion.div>
            )}
          </AnimatePresence>
        </div>

        {/* Virtual Interaction */}
        <div className="grid grid-cols-1 md:grid-cols-2 gap-6 w-full mb-12">
          <motion.div 
            whileHover={{ y: -5 }}
            className="bg-white/80 p-6 rounded-2xl text-center border border-rose-50"
          >
            <Coffee className="mx-auto mb-3 text-amber-500" />
            <h3 className="font-bold mb-2">Virtual Coffee Date</h3>
            <p className="text-sm text-slate-500 mb-4">Let's hop on a call and just exist together.</p>
            <button className="text-rose-500 text-sm font-bold hover:underline">Schedule Time →</button>
          </motion.div>

          <motion.div 
            whileHover={{ y: -5 }}
            className="bg-white/80 p-6 rounded-2xl text-center border border-rose-50"
          >
            <Gift className="mx-auto mb-3 text-rose-400" />
            <h3 className="font-bold mb-2">Send a Virtual Hug</h3>
            <p className="text-sm text-slate-500 mb-4">Hugs sent: {hugsSent}</p>
            <button 
              onClick={() => setHugsSent(s => s + 1)}
              className="bg-rose-100 text-rose-600 px-4 py-2 rounded-full text-xs font-bold hover:bg-rose-200 transition-colors"
            >
              Click to Hug!
            </button>
          </motion.div>
        </div>

        {/* Surprise Section */}
        <div className="w-full text-center">
          <button 
            onClick={() => setShowSurprise(true)}
            className="text-rose-300 hover:text-rose-500 transition-colors flex items-center justify-center gap-2 mx-auto"
          >
            <Stars size={16} />
            <span className="text-sm font-medium">A tiny surprise for you</span>
          </button>

          <AnimatePresence>
            {showSurprise && (
              <motion.div
                initial={{ scale: 0 }}
                animate={{ scale: 1 }}
                className="fixed inset-0 z-50 flex items-center justify-center p-6 bg-rose-900/20 backdrop-blur-sm"
                onClick={() => setShowSurprise(false)}
              >
                <motion.div 
                  className="bg-white p-10 rounded-3xl shadow-2xl max-w-sm text-center relative"
                  onClick={e => e.stopPropagation()}
                >
                  <div className="text-5xl mb-4">🎁</div>
                  <h2 className="text-2xl font-bold text-rose-600 mb-4">I Love You!</h2>
                  <p className="text-slate-600 mb-6">
                    I've hidden a real gift in your mail/inbox! Go check it when you can. ❤️
                  </p>
                  <button 
                    onClick={() => setShowSurprise(false)}
                    className="bg-rose-500 text-white px-6 py-2 rounded-full font-bold"
                  >
                    Yay!
                  </button>
                </motion.div>
              </motion.div>
            )}
          </AnimatePresence>
        </div>

        {/* Footer */}
        <footer className="mt-20 text-center text-rose-300 text-xs tracking-widest uppercase">
          <div className="flex items-center justify-center gap-4 mb-4">
            <MapPin size={12} />
            <span>Your Heart</span>
            <div className="h-px w-8 bg-rose-200"></div>
            <Heart size={12} fill="currentColor" />
            <div className="h-px w-8 bg-rose-200"></div>
            <span>My Heart</span>
          </div>
          Made with love for my favorite person
        </footer>
      </main>
    </div>
  );
};

export default ValentineWebsite;
