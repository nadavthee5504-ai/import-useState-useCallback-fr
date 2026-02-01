import { useState, useCallback } from 'react';
import { motion, AnimatePresence } from 'framer-motion';
import { BookOpen, Zap, Plus, Sparkles, Brain } from 'lucide-react';
import { Button } from '@/components/ui/button';
import { BulkImport } from '@/components/BulkImport';
import { SpeedMatchGame } from '@/components/SpeedMatchGame';
import { WordPair } from '@/types';

type AppView = 'home' | 'import' | 'game';

const Index = () => {
  const [currentView, setCurrentView] = useState<AppView>('home');
  const [words, setWords] = useState<WordPair[]>([]);

  const handleImportComplete = useCallback((importedWords: WordPair[]) => {
    setWords(importedWords);
    setCurrentView('game');
  }, []);

  const handleExitGame = useCallback(() => {
    setCurrentView('home');
  }, []);

  return (
    <div className="min-h-screen bg-background">
      {/* Decorative background elements */}
      <div className="fixed inset-0 overflow-hidden pointer-events-none">
        <div className="absolute -top-40 -right-40 w-96 h-96 rounded-full bg-primary/5 blur-3xl" />
        <div className="absolute -bottom-40 -left-40 w-96 h-96 rounded-full bg-success/5 blur-3xl" />
        <div className="absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2 w-[800px] h-[800px] rounded-full bg-accent/5 blur-3xl" />
      </div>

      <div className="relative z-10">
        {/* Header */}
        <header className="py-6 px-4 md:px-8">
          <div className="max-w-6xl mx-auto flex items-center justify-between">
            <motion.div 
              initial={{ opacity: 0, x: -20 }}
              animate={{ opacity: 1, x: 0 }}
              className="flex items-center gap-3"
            >
              <div className="w-12 h-12 rounded-2xl gradient-primary flex items-center justify-center shadow-lg">
                <Brain className="h-7 w-7 text-primary-foreground" />
              </div>
              <div>
                <h1 className="text-xl font-bold text-foreground">WordMatch</h1>
                <p className="text-xs text-muted-foreground">Learn English & Hebrew</p>
              </div>
            </motion.div>

            {currentView === 'home' && (
              <motion.div
                initial={{ opacity: 0, x: 20 }}
                animate={{ opacity: 1, x: 0 }}
              >
                <Button 
                  onClick={() => setCurrentView('import')}
                  className="gradient-primary text-primary-foreground hover:opacity-90"
                >
                  <Plus className="h-4 w-4 mr-2" />
                  New Set
                </Button>
              </motion.div>
            )}
          </div>
        </header>

        {/* Main Content */}
        <main className="px-4 md:px-8 pb-12">
          <AnimatePresence mode="wait">
            {/* Home View */}
            {currentView === 'home' && (
              <motion.div
                key="home"
                initial={{ opacity: 0, y: 20 }}
                animate={{ opacity: 1, y: 0 }}
                exit={{ opacity: 0, y: -20 }}
                className="max-w-4xl mx-auto"
              >
                {/* Hero Section */}
                <div className="text-center py-12 md:py-20">
                  <motion.div
                    animate={{ y: [0, -10, 0] }}
                    transition={{ repeat: Infinity, duration: 3, ease: 'easeInOut' }}
                    className="inline-flex items-center justify-center w-24 h-24 rounded-3xl gradient-game mb-8 shadow-lg"
                  >
                    <Sparkles className="h-12 w-12 text-primary-foreground" />
                  </motion.div>

                  <h2 className="text-4xl md:text-5xl lg:text-6xl font-extrabold mb-6">
                    <span className="text-foreground">Master Vocabulary</span>
                    <br />
                    <span className="text-gradient-primary">The Fun Way</span>
                  </h2>

                  <p className="text-lg md:text-xl text-muted-foreground max-w-2xl mx-auto mb-10">
                    Import your word lists and learn English-Hebrew vocabulary through 
                    engaging speed-matching games. Track your progress and beat your high scores!
                  </p>

                  <div className="flex flex-col sm:flex-row items-center justify-center gap-4">
                    <Button 
                      size="lg"
                      onClick={() => setCurrentView('import')}
                      className="gradient-primary text-primary-foreground hover:opacity-90 px-8 py-6 text-lg rounded-2xl shadow-lg"
                    >
                      <BookOpen className="h-5 w-5 mr-2" />
                      Import Word List
                    </Button>
                    
                    {words.length > 0 && (
                      <Button 
                        size="lg"
                        variant="outline"
                        onClick={() => setCurrentView('game')}
                        className="px-8 py-6 text-lg rounded-2xl"
                      >
                        <Zap className="h-5 w-5 mr-2" />
                        Continue Playing
                      </Button>
                    )}
                  </div>
                </div>

                {/* Features */}
                <div className="grid grid-cols-1 md:grid-cols-3 gap-6 mt-8">
                  <motion.div
                    initial={{ opacity: 0, y: 20 }}
                    animate={{ opacity: 1, y: 0 }}
                    transition={{ delay: 0.1 }}
                    className="bg-card rounded-3xl p-6 shadow-card"
                  >
                    <div className="w-14 h-14 rounded-2xl bg-primary/10 flex items-center justify-center mb-4">
                      <BookOpen className="h-7 w-7 text-primary" />
                    </div>
                    <h3 className="text-lg font-bold text-foreground mb-2">Easy Import</h3>
                    <p className="text-muted-foreground">
                      Paste your word list or upload a file. We'll parse it automatically.
                    </p>
                  </motion.div>

                  <motion.div
                    initial={{ opacity: 0, y: 20 }}
                    animate={{ opacity: 1, y: 0 }}
                    transition={{ delay: 0.2 }}
                    className="bg-card rounded-3xl p-6 shadow-card"
                  >
                    <div className="w-14 h-14 rounded-2xl bg-success/10 flex items-center justify-center mb-4">
                      <Zap className="h-7 w-7 text-success" />
                    </div>
                    <h3 className="text-lg font-bold text-foreground mb-2">Speed Match</h3>
                    <p className="text-muted-foreground">
                      Race against the clock to match words with their translations.
                    </p>
                  </motion.div>

                  <motion.div
                    initial={{ opacity: 0, y: 20 }}
                    animate={{ opacity: 1, y: 0 }}
                    transition={{ delay: 0.3 }}
                    className="bg-card rounded-3xl p-6 shadow-card"
                  >
                    <div className="w-14 h-14 rounded-2xl bg-accent/10 flex items-center justify-center mb-4">
                      <Sparkles className="h-7 w-7 text-accent" />
                    </div>
                    <h3 className="text-lg font-bold text-foreground mb-2">Gamified Learning</h3>
                    <p className="text-muted-foreground">
                      Earn points, beat your high scores, and celebrate victories with confetti!
                    </p>
                  </motion.div>
                </div>

                {/* Current Words */}
                {words.length > 0 && (
                  <motion.div
                    initial={{ opacity: 0, y: 20 }}
                    animate={{ opacity: 1, y: 0 }}
                    transition={{ delay: 0.4 }}
                    className="mt-12 bg-card rounded-3xl p-6 shadow-card"
                  >
                    <div className="flex items-center justify-between mb-4">
                      <h3 className="text-lg font-bold text-foreground">Current Word Set</h3>
                      <span className="px-3 py-1 rounded-full bg-primary/10 text-primary text-sm font-semibold">
                        {words.length} words
                      </span>
                    </div>
                    <div className="flex flex-wrap gap-2">
                      {words.slice(0, 10).map((word) => (
                        <span
                          key={word.id}
                          className="px-3 py-1.5 rounded-lg bg-muted text-sm text-muted-foreground"
                        >
                          {word.english}
                        </span>
                      ))}
                      {words.length > 10 && (
                        <span className="px-3 py-1.5 rounded-lg bg-muted text-sm text-muted-foreground">
                          +{words.length - 10} more
                        </span>
                      )}
                    </div>
                  </motion.div>
                )}
              </motion.div>
            )}

            {/* Import View */}
            {currentView === 'import' && (
              <motion.div
                key="import"
                initial={{ opacity: 0, y: 20 }}
                animate={{ opacity: 1, y: 0 }}
                exit={{ opacity: 0, y: -20 }}
                className="py-8"
              >
                <BulkImport 
                  onImport={handleImportComplete}
                  onCancel={() => setCurrentView('home')}
                />
              </motion.div>
            )}

            {/* Game View */}
            {currentView === 'game' && words.length > 0 && (
              <motion.div
                key="game"
                initial={{ opacity: 0, scale: 0.98 }}
                animate={{ opacity: 1, scale: 1 }}
                exit={{ opacity: 0, scale: 0.98 }}
                className="py-8"
              >
                <SpeedMatchGame 
                  words={words}
                  onExit={handleExitGame}
                />
              </motion.div>
            )}
          </AnimatePresence>
        </main>
      </div>
    </div>
  );
};

export default Index;
