import React, { useState, useEffect } from 'react';
import { motion, AnimatePresence } from 'framer-motion';
import { Calendar, Clock, CheckSquare, Trash2, Plus, Moon, Sun } from 'lucide-react';

// РџСЂРёР»РѕР¶РµРЅРёРµ "Р•Р¶РµРґРЅРµРІРЅРёРє"
const DarkPlanner = () => {
  // РџРѕР»СѓС‡РµРЅРёРµ С‚РµРєСѓС‰РµР№ РґР°С‚С‹ Рё РІСЂРµРјРµРЅРё
  const [currentTime, setCurrentTime] = useState(new Date());
  const [tasks, setTasks] = useState([
    { id: 1, text: 'Р’СЃС‚СЂРµС‡Р° СЃ РєР»РёРµРЅС‚РѕРј', completed: false, time: '10:00' },
    { id: 2, text: 'РћР±РµРґ СЃ РєРѕР»Р»РµРіР°РјРё', completed: true, time: '13:00' },
    { id: 3, text: 'Р—Р°РєРѕРЅС‡РёС‚СЊ РїСЂРµР·РµРЅС‚Р°С†РёСЋ', completed: false, time: '16:30' }
  ]);
  const [newTask, setNewTask] = useState('');
  const [newTaskTime, setNewTaskTime] = useState('');
  const [showForm, setShowForm] = useState(false);
  const [darkMode, setDarkMode] = useState(true);

  // РћР±РЅРѕРІР»РµРЅРёРµ РІСЂРµРјРµРЅРё РєР°Р¶РґСѓСЋ СЃРµРєСѓРЅРґСѓ
  useEffect(() => {
    const timer = setInterval(() => {
      setCurrentTime(new Date());
    }, 1000);
    
    return () => clearInterval(timer);
  }, []);

  // Р¤РѕСЂРјР°С‚РёСЂРѕРІР°РЅРёРµ С‚РµРєСѓС‰РµР№ РґР°С‚С‹
  const formatDate = (date) => {
    const options = { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' };
    return date.toLocaleDateString('ru-RU', options);
  };

  // Р¤РѕСЂРјР°С‚РёСЂРѕРІР°РЅРёРµ С‚РµРєСѓС‰РµРіРѕ РІСЂРµРјРµРЅРё
  const formatTime = (date) => {
    return date.toLocaleTimeString('ru-RU', { hour: '2-digit', minute: '2-digit', second: '2-digit' });
  };

  // Р”РѕР±Р°РІР»РµРЅРёРµ РЅРѕРІРѕР№ Р·Р°РґР°С‡Рё
  const addTask = () => {
    if (newTask.trim() !== '') {
      const newTaskObj = {
        id: Date.now(),
        text: newTask,
        completed: false,
        time: newTaskTime || '00:00'
      };
      
      setTasks([...tasks, newTaskObj]);
      setNewTask('');
      setNewTaskTime('');
      setShowForm(false);
    }
  };

  // РџРµСЂРµРєР»СЋС‡РµРЅРёРµ СЃС‚Р°С‚СѓСЃР° Р·Р°РґР°С‡Рё
  const toggleTask = (id) => {
    setTasks(tasks.map(task => 
      task.id === id ? { ...task, completed: !task.completed } : task
    ));
  };

  // РЈРґР°Р»РµРЅРёРµ Р·Р°РґР°С‡Рё
  const deleteTask = (id) => {
    setTasks(tasks.filter(task => task.id !== id));
  };

  // РџРµСЂРµРєР»СЋС‡РµРЅРёРµ РјРµР¶РґСѓ СЃРІРµС‚Р»РѕР№ Рё С‚РµРјРЅРѕР№ С‚РµРјРѕР№
  const toggleTheme = () => {
    setDarkMode(!darkMode);
  };

  // Р’Р°СЂРёР°РЅС‚С‹ Р°РЅРёРјР°С†РёРё РґР»СЏ СЌР»РµРјРµРЅС‚РѕРІ
  const containerVariants = {
    hidden: { opacity: 0 },
    visible: { 
      opacity: 1,
      transition: { staggerChildren: 0.1 }
    }
  };

  const itemVariants = {
    hidden: { y: 20, opacity: 0 },
    visible: { 
      y: 0, 
      opacity: 1,
      transition: { type: 'spring', stiffness: 300 }
    }
  };

  const themeClass = darkMode ? 'bg-gray-900 text-white' : 'bg-gray-100 text-gray-800';
  const cardClass = darkMode ? 'bg-gray-800' : 'bg-white';
  const buttonClass = darkMode ? 'bg-indigo-600 hover:bg-indigo-700' : 'bg-indigo-500 hover:bg-indigo-600';
  const inputClass = darkMode ? 'bg-gray-700 text-white border-gray-600' : 'bg-white text-gray-800 border-gray-300';

  return (
    <div className={`min-h-screen p-4 transition-colors duration-500 ${themeClass}`}>
      <div className="max-w-md mx-auto">
        {/* РЁР°РїРєР° СЃ РґР°С‚РѕР№ Рё РІСЂРµРјРµРЅРµРј */}
        <motion.div 
          initial={{ y: -50, opacity: 0 }}
          animate={{ y: 0, opacity: 1 }}
          transition={{ type: 'spring', stiffness: 100 }}
          className={`p-6 rounded-lg shadow-lg mb-6 ${cardClass}`}
        >
          <div className="flex justify-between items-center mb-4">
            <h1 className="text-2xl font-bold">РњРѕР№ РµР¶РµРґРЅРµРІРЅРёРє</h1>
            <motion.button 
              whileTap={{ scale: 0.9 }}
              onClick={toggleTheme}
              className="p-2 rounded-full"
            >
              {darkMode ? <Sun size={20} /> : <Moon size={20} />}
            </motion.button>
          </div>
          
          <div className="flex items-center mb-2">
            <Calendar className="mr-2" size={20} />
            <span className="text-md">{formatDate(currentTime)}</span>
          </div>
          
          <div className="flex items-center">
            <Clock className="mr-2" size={20} />
            <motion.span 
              key={currentTime.getSeconds()}
              initial={{ opacity: 0 }}
              animate={{ opacity: 1 }}
              className="text-md font-mono"
            >
              {formatTime(currentTime)}
            </motion.span>
          </div>
        </motion.div>
        
        {/* РЎРїРёСЃРѕРє Р·Р°РґР°С‡ */}
        <motion.div 
          className={`p-6 rounded-lg shadow-lg mb-6 ${cardClass}`}
          variants={containerVariants}
          initial="hidden"
          animate="visible"
        >
          <h2 className="text-xl font-bold mb-4">Р—Р°РґР°С‡Рё РЅР° СЃРµРіРѕРґРЅСЏ</h2>
          
          <AnimatePresence>
            {tasks.map(task => (
              <motion.div 
                key={task.id}
                variants={itemVariants}
                exit={{ opacity: 0, x: -100 }}
                className={`p-3 mb-3 rounded-md ${darkMode ? 'bg-gray-700' : 'bg-gray-100'} flex justify-between items-center`}
              >
                <div className="flex items-center">
                  <motion.button
                    whileTap={{ scale: 0.9 }}
                    onClick={() => toggleTask(task.id)}
                    className={`mr-3 flex-shrink-0 w-6 h-6 rounded-md border ${task.completed ? (darkMode ? 'bg-indigo-600 border-indigo-600' : 'bg-indigo-500 border-indigo-500') : 'border-gray-500'} flex items-center justify-center`}
                  >
                    {task.completed && <CheckSquare size={16} className="text-white" />}
                  </motion.button>
                  <div>
                    <p className={`${task.completed ? 'line-through opacity-50' : ''}`}>{task.text}</p>
                    <p className="text-xs opacity-70">{task.time}</p>
                  </div>
                </div>
                <motion.button
                  whileHover={{ scale: 1.1 }}
                  whileTap={{ scale: 0.9 }}
                  onClick={() => deleteTask(task.id)}
                  className="text-red-500"
                >
                  <Trash2 size={18} />
                </motion.button>
              </motion.div>
            ))}
          </AnimatePresence>
          
          {tasks.length === 0 && (
            <motion.p 
              initial={{ opacity: 0 }}
              animate={{ opacity: 0.6 }}
              className="text-center py-4"
            >
              РќРµС‚ Р·Р°РґР°С‡. Р”РѕР±Р°РІСЊС‚Рµ РЅРѕРІСѓСЋ Р·Р°РґР°С‡Сѓ!
            </motion.p>
          )}
          
          {/* РљРЅРѕРїРєР° РґРѕР±Р°РІР»РµРЅРёСЏ Р·Р°РґР°С‡Рё */}
          <motion.button
            whileHover={{ scale: 1.05 }}
            whileTap={{ scale: 0.95 }}
            onClick={() => setShowForm(true)}
            className={`mt-4 w-full py-2 rounded-md ${buttonClass} text-white flex items-center justify-center`}
          >
            <Plus size={18} className="mr-2" />
            Р”РѕР±Р°РІРёС‚СЊ Р·Р°РґР°С‡Сѓ
          </motion.button>
        </motion.div>
        
        {/* Р¤РѕСЂРјР° РґРѕР±Р°РІР»РµРЅРёСЏ Р·Р°РґР°С‡Рё */}
        <AnimatePresence>
          {showForm && (
            <motion.div
              initial={{ opacity: 0, y: 20 }}
              animate={{ opacity: 1, y: 0 }}
              exit={{ opacity: 0, y: 20 }}
              className={`p-6 rounded-lg shadow-lg ${cardClass}`}
            >
              <h2 className="text-xl font-bold mb-4">РќРѕРІР°СЏ Р·Р°РґР°С‡Р°</h2>
              
              <div className="mb-4">
                <label className="block mb-2 text-sm">РќР°Р·РІР°РЅРёРµ Р·Р°РґР°С‡Рё</label>
                <input
                  type="text"
                  value={newTask}
                  onChange={(e) => setNewTask(e.target.value)}
                  className={`w-full p-2 rounded-md ${inputClass} border`}
                  placeholder="Р’РІРµРґРёС‚Рµ Р·Р°РґР°С‡Сѓ..."
                />
              </div>
              
              <div className="mb-4">
                <label className="block mb-2 text-sm">Р’СЂРµРјСЏ</label>
                <input
                  type="time"
                  value={newTaskTime}
                  onChange={(e) => setNewTaskTime(e.target.value)}
                  className={`w-full p-2 rounded-md ${inputClass} border`}
                />
              </div>
              
              <div className="flex space-x-2">
                <motion.button
                  whileHover={{ scale: 1.05 }}
                  whileTap={{ scale: 0.95 }}
                  onClick={addTask}
                  className={`flex-1 py-2 rounded-md ${buttonClass} text-white`}
                >
                  РЎРѕС…СЂР°РЅРёС‚СЊ
                </motion.button>
                <motion.button
                  whileHover={{ scale: 1.05 }}
                  whileTap={{ scale: 0.95 }}
                  onClick={() => setShowForm(false)}
                  className={`flex-1 py-2 rounded-md ${darkMode ? 'bg-gray-700 hover:bg-gray-600' : 'bg-gray-200 hover:bg-gray-300'}`}
                >
                  РћС‚РјРµРЅР°
                </motion.button>
              </div>
            </motion.div>
          )}
        </AnimatePresence>
      </div>
    </div>
  );
};

export default DarkPlanner;
